<script>
  import nimg from "./assets/n_med.jpeg";
  import * as THREE from "jsr:@3d/three";
  import { onMount } from "svelte";

  // variables
  let imageContainer;
  let imageElement;
  let easeFactor = 0.02;
  let scene, camera, renderer, planeMesh;
  let mousePosition = { x: 0.5, y: 0.5 };
  let targetMousePosition = { x: 0.5, y: 0.5 };
  let mouseStopTimeout;
  let aberrationIntensity = 0.0;
  let lastPosition = { x: 0.5, y: 0.5 };
  let prevPosition = { x: 0.5, y: 0.5 };

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

  function initializeScene(texture) {
    scene = new THREE.Scene();

    camera = new THREE.PerspectiveCamera(
      90,
      imageElement.offsetWidth / imageElement.offsetHeight,
      0.01,
      10
    );
    camera.position.z = 1;

    const shaderUniforms = {
      u_mouse: { type: "v2", value: new THREE.Vector2() },
      u_prevMouse: { type: "v2", value: new THREE.Vector2() },
      u_aberrationIntensity: { type: "f", value: 0.0 },
      u_texture: { type: "t", value: texture },
    };

    planeMesh = new THREE.Mesh(
      new THREE.PlaneGeometry(2, 2),
      new THREE.ShaderMaterial({
        uniforms: shaderUniforms,
        vertexShader,
        fragmentShader,
      })
    );

    scene.add(planeMesh);

    renderer = new THREE.WebGLRenderer({ alpha: true }); // Added alpha: true for transparency
    renderer.setSize(imageElement.offsetWidth, imageElement.offsetHeight);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.domElement.style.imageRendering = "pixelated";

    // Remove existing canvas if it exists
    const existingCanvas = imageContainer.querySelector("canvas");
    if (existingCanvas) {
      imageContainer.removeChild(existingCanvas);
    }

    imageContainer.appendChild(renderer.domElement);
  }

  function animateScene() {
    requestAnimationFrame(animateScene);

    mousePosition.x += (targetMousePosition.x - mousePosition.x) * easeFactor;
    mousePosition.y += (targetMousePosition.y - mousePosition.y) * easeFactor;

    planeMesh.material.uniforms.u_mouse.value.set(
      mousePosition.x,
      1.0 - mousePosition.y
    );

    planeMesh.material.uniforms.u_prevMouse.value.set(
      prevPosition.x,
      1.0 - prevPosition.y
    );

    aberrationIntensity = Math.max(0.0, aberrationIntensity - 0.05);
    planeMesh.material.uniforms.u_aberrationIntensity.value =
      aberrationIntensity;

    renderer.render(scene, camera);
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

  onMount(() => {
    imageContainer = document.getElementById("imageContainer");
    imageElement = document.getElementById("myImage");

    if (!imageElement) return;

    // Wait for image to load before initializing
    imageElement.onload = () => {
      const texture = new THREE.TextureLoader().load(imageElement.src);
      texture.magFilter = THREE.NearestFilter;
      texture.minFilter = THREE.LinearFilter;

      initializeScene(texture);
      animateScene();

      // Hide the original image once Three.js scene is initialized
      imageElement.style.display = "none";

      imageContainer.addEventListener("mousemove", handleMouseMove);
      imageContainer.addEventListener("mouseenter", handleMouseEnter);
      imageContainer.addEventListener("mouseleave", handleMouseLeave);
    };

    return () => {
      if (imageContainer) {
        imageContainer.removeEventListener("mousemove", handleMouseMove);
        imageContainer.removeEventListener("mouseenter", handleMouseEnter);
        imageContainer.removeEventListener("mouseleave", handleMouseLeave);
      }
      if (renderer) {
        renderer.dispose();
      }
    };
  });
</script>

<div id="imageContainer" bind:this={imageContainer}>
  <img src={nimg} alt="Nitish" id="myImage" bind:this={imageElement} />
  <!-- <img src={nimg} /> -->
</div>

<style>
  canvas {
    display: block;
  }

  #imageContainer {
    position: relative;
    width: 320px;
    height: 320px;
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
