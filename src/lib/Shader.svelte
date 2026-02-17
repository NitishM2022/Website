<script>
  import * as THREE from "three";
  import { onMount } from "svelte";

  // variables
  let imageContainer;
  let imageElement;
  let easeFactor = 0.02;
  let scene, camera, renderer, planeMesh;
  let geometry;
  let material;
  let texture;
  let mousePosition = { x: 0.5, y: 0.5 };
  let targetMousePosition = { x: 0.5, y: 0.5 };
  let mouseStopTimeout;
  let aberrationIntensity = 0.0;
  let prevPosition = { x: 0.5, y: 0.5 };
  let rafId = 0;
  let isMounted = false;
  let sceneInitialized = false;
  let isVisible = true;

  // shaders
  const vertexShader = `
      varying vec2 vUv;
      void main() {
        vUv = uv;
        gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
      }
  `;

  const fragmentShader = `
      varying vec2 vUv;
      uniform sampler2D u_texture;    
      uniform vec2 u_mouse;
      uniform vec2 u_prevMouse;
      uniform float u_aberrationIntensity;

      void main() {
          vec2 gridUV = floor(vUv * vec2(20.0, 20.0)) / vec2(20.0, 20.0);
          vec2 centerOfPixel = gridUV + vec2(1.0/20.0, 1.0/20.0);
          
          vec2 mouseDirection = u_mouse - u_prevMouse;
          
          vec2 pixelToMouseDirection = centerOfPixel - u_mouse;
          float pixelDistanceToMouse = length(pixelToMouseDirection);
          float strength = smoothstep(0.3, 0.0, pixelDistanceToMouse);
  
          vec2 uvOffset = strength * - mouseDirection * 0.2;
          vec2 uv = vUv - uvOffset;

          vec4 colorR = texture2D(u_texture, uv + vec2(strength * u_aberrationIntensity * 0.01, 0.0));
          vec4 colorG = texture2D(u_texture, uv);
          vec4 colorB = texture2D(u_texture, uv - vec2(strength * u_aberrationIntensity * 0.01, 0.0));

          gl_FragColor = vec4(colorR.r, colorG.g, colorB.b, 1.0);
      }
  `;

  function initializeScene(loadedTexture) {
    scene = new THREE.Scene();

    camera = new THREE.PerspectiveCamera(
      90,
      imageElement.offsetWidth / imageElement.offsetHeight,
      0.01,
      10,
    );
    camera.position.z = 1;

    const shaderUniforms = {
      u_mouse: { type: "v2", value: new THREE.Vector2() },
      u_prevMouse: { type: "v2", value: new THREE.Vector2() },
      u_aberrationIntensity: { type: "f", value: 0.0 },
      u_texture: { type: "t", value: loadedTexture },
    };

    geometry = new THREE.PlaneGeometry(2, 2);
    material = new THREE.ShaderMaterial({
      uniforms: shaderUniforms,
      vertexShader,
      fragmentShader,
    });

    planeMesh = new THREE.Mesh(geometry, material);

    scene.add(planeMesh);

    renderer = new THREE.WebGLRenderer({ alpha: true }); // Added alpha: true for transparency
    renderer.setSize(imageElement.offsetWidth, imageElement.offsetHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 2));
    renderer.domElement.style.imageRendering = "pixelated";

    // Remove existing canvas if it exists
    const existingCanvas = imageContainer.querySelector("canvas");
    if (existingCanvas) {
      imageContainer.removeChild(existingCanvas);
    }

    imageContainer.appendChild(renderer.domElement);
  }

  function stopAnimation() {
    if (rafId) {
      cancelAnimationFrame(rafId);
      rafId = 0;
    }
  }

  function startAnimation() {
    if (!isMounted || !sceneInitialized || !isVisible || rafId) return;
    rafId = requestAnimationFrame(animateScene);
  }

  function animateScene() {
    if (!isMounted || !sceneInitialized || !isVisible) {
      stopAnimation();
      return;
    }

    rafId = requestAnimationFrame(animateScene);

    mousePosition.x += (targetMousePosition.x - mousePosition.x) * easeFactor;
    mousePosition.y += (targetMousePosition.y - mousePosition.y) * easeFactor;

    planeMesh.material.uniforms.u_mouse.value.set(
      mousePosition.x,
      1.0 - mousePosition.y,
    );

    planeMesh.material.uniforms.u_prevMouse.value.set(
      prevPosition.x,
      1.0 - prevPosition.y,
    );

    aberrationIntensity = Math.max(0.0, aberrationIntensity - 0.05);
    planeMesh.material.uniforms.u_aberrationIntensity.value =
      aberrationIntensity;

    renderer.render(scene, camera);
  }

  function handleVisibilityChange() {
    isVisible = !document.hidden;
    if (isVisible) {
      startAnimation();
    } else {
      stopAnimation();
    }
  }

  function handleMouseMove(event) {
    easeFactor = 0.02;
    const rect = imageContainer.getBoundingClientRect();
    prevPosition = { ...targetMousePosition };

    targetMousePosition.x = (event.clientX - rect.left) / rect.width;
    targetMousePosition.y = (event.clientY - rect.top) / rect.height;

    aberrationIntensity = 1;
  }

  function handleMouseEnter(event) {
    easeFactor = 0.02;
    const rect = imageContainer.getBoundingClientRect();

    mousePosition.x = targetMousePosition.x =
      (event.clientX - rect.left) / rect.width;
    mousePosition.y = targetMousePosition.y =
      (event.clientY - rect.top) / rect.height;
  }

  function handleMouseLeave() {
    easeFactor = 0.05;
    targetMousePosition = { ...prevPosition };
  }

  function startShader() {
    if (!imageElement || sceneInitialized || !isMounted) return;

    texture = new THREE.TextureLoader().load(imageElement.src);
    texture.magFilter = THREE.NearestFilter;
    texture.minFilter = THREE.LinearFilter;

    initializeScene(texture);
    sceneInitialized = true;
    startAnimation();

    // Hide the original image once Three.js scene is initialized
    imageElement.style.display = "none";

    imageContainer.addEventListener("mousemove", handleMouseMove);
    imageContainer.addEventListener("mouseenter", handleMouseEnter);
    imageContainer.addEventListener("mouseleave", handleMouseLeave);
  }

  onMount(() => {
    isMounted = true;
    if (!imageElement) return;
    isVisible = !document.hidden;
    document.addEventListener("visibilitychange", handleVisibilityChange);

    // Wait for image to load before initializing
    if (imageElement.complete) {
      startShader();
    } else {
      imageElement.onload = startShader;
    }

    return () => {
      isMounted = false;
      stopAnimation();
      document.removeEventListener("visibilitychange", handleVisibilityChange);

      if (imageContainer) {
        imageContainer.removeEventListener("mousemove", handleMouseMove);
        imageContainer.removeEventListener("mouseenter", handleMouseEnter);
        imageContainer.removeEventListener("mouseleave", handleMouseLeave);
      }
      if (mouseStopTimeout) {
        clearTimeout(mouseStopTimeout);
      }
      if (imageElement) {
        imageElement.onload = null;
      }
      if (planeMesh) {
        scene?.remove(planeMesh);
      }
      geometry?.dispose();
      material?.dispose();
      texture?.dispose();
      if (renderer) {
        const canvas = renderer.domElement;
        if (canvas?.parentNode) {
          canvas.parentNode.removeChild(canvas);
        }
        renderer.dispose();
        renderer.forceContextLoss?.();
      }

      planeMesh = undefined;
      geometry = undefined;
      material = undefined;
      texture = undefined;
      renderer = undefined;
      scene = undefined;
      camera = undefined;
      sceneInitialized = false;
    };
  });
</script>

<div
  id="imageContainer"
  class="rounded-tr-2xl rounded-bl-2xl"
  bind:this={imageContainer}
>
  <img src="./n_med.jpeg" alt="Nitish" id="myImage" bind:this={imageElement} />
  <!-- <img src={nimg} /> -->
</div>

<style>
  canvas {
    display: block;
  }

  #imageContainer {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    max-width: 100%;
  }

  #imageContainer > * {
    position: absolute;
    inset: 0;
    width: 100% !important;
    height: 100% !important;
    object-fit: contain;
  }
</style>
