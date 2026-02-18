<script>
  import { onMount } from "svelte";
  import * as THREE from "three";

  export let text = "howdy";
  export let asciiFontSize = 8;
  export let textFontSize = 200;
  export let textColor = "#fdf9f3";
  export let planeBaseHeight = 8;
  export let enableWaves = true;
  export let skewAngle = -12;

  let container;
  let asciiInstance;
  let containerWidth = 0;
  let containerHeight = 0;
  let fontsLoaded = false;

  // Update ASCII instance when container dimensions change
  $: if (asciiInstance && containerWidth > 0 && containerHeight > 0) {
    asciiInstance.setSize(containerWidth, containerHeight);
  }

  const vertexShader = `
        varying vec2 vUv;
        uniform float uTime;
        uniform float mouse;
        uniform float uEnableWaves;

        void main() {
            vUv = uv;
            float time = uTime * 5.;
            float waveFactor = uEnableWaves;
            vec3 transformed = position;

            transformed.x += sin(time + position.y) * 0.5 * waveFactor;
            transformed.y += cos(time + position.z) * 0.15 * waveFactor;
            transformed.z += sin(time + position.x) * waveFactor;

            gl_Position = projectionMatrix * modelViewMatrix * vec4(transformed, 1.0);
        }
    `;

  const fragmentShader = `
        varying vec2 vUv;
        uniform float mouse;
        uniform float uTime;
        uniform sampler2D uTexture;

        void main() {
            float time = uTime;
            vec2 pos = vUv;
            
            float r = texture2D(uTexture, pos + cos(time * 2. - time + pos.x) * .01).r;
            float g = texture2D(uTexture, pos + tan(time * .5 + pos.x - time) * .01).g;
            float b = texture2D(uTexture, pos - cos(time * 2. + time + pos.y) * .01).b;
            float a = texture2D(uTexture, pos).a;
            gl_FragColor = vec4(r, g, b, a);
        }
    `;

  const PX_RATIO = typeof window !== "undefined" ? window.devicePixelRatio : 1;

  Math.map = function (n, start, stop, start2, stop2) {
    return ((n - start) / (stop - start)) * (stop2 - start2) + start2;
  };

  class AsciiFilter {
    constructor(renderer, { fontSize, fontFamily, charset, invert } = {}) {
      this.renderer = renderer;
      this.domElement = document.createElement("div");
      this.domElement.style.position = "absolute";
      this.domElement.style.top = "0";
      this.domElement.style.left = "0";
      this.domElement.style.width = "100%";
      this.domElement.style.height = "100%";

      this.pre = document.createElement("pre");
      this.domElement.appendChild(this.pre);

      this.canvas = document.createElement("canvas");
      this.context = this.canvas.getContext("2d");
      this.domElement.appendChild(this.canvas);

      this.deg = 0;
      this.invert = invert ?? true;
      this.fontSize = fontSize ?? 12;
      this.fontFamily = fontFamily ?? "'Courier New', monospace";
      this.charset =
        charset ??
        " .'`^\",:;Il!i~+_-?][}{1)(|/tfjrxnuvczXYUJCLQ0OZmwqpdbkhao*#MW&8%B@$";

      this.context.imageSmoothingEnabled = false;

      this.onMouseMove = this.onMouseMove.bind(this);
      document.addEventListener("mousemove", this.onMouseMove);

      // Track if we've measured the font yet
      this.fontMeasured = false;
    }

    setSize(width, height) {
      this.width = width;
      this.height = height;
      this.renderer.setSize(width, height);
      this.reset();
      this.center = { x: width / 2, y: height / 2 };
      this.mouse = { x: this.center.x, y: this.center.y };
    }

    reset() {
      // Set font and measure character dimensions
      this.context.font = `600 ${this.fontSize}px ${this.fontFamily}`;

      // Measure actual character width - use a wider character for more accurate measurement
      const charWidth = this.context.measureText("M").width;

      // Calculate columns and rows based on actual measurements
      this.cols = Math.floor(this.width / charWidth);
      this.rows = Math.floor(this.height / this.fontSize);

      this.canvas.width = this.cols;
      this.canvas.height = this.rows;

      // Apply exact same font settings to the pre element
      this.pre.style.fontFamily = this.fontFamily;
      this.pre.style.fontSize = `${this.fontSize}px`;
      this.pre.style.fontWeight = "600";
      this.pre.style.margin = "0";
      this.pre.style.padding = "0";
      this.pre.style.lineHeight = "1em";
      this.pre.style.position = "absolute";
      this.pre.style.left = "0";
      this.pre.style.top = "0";
      this.pre.style.width = "100%";
      this.pre.style.height = "100%";
      this.pre.style.zIndex = "9";
      this.pre.style.backgroundAttachment = "fixed";
      this.pre.style.mixBlendMode = "difference";
      // Disable text wrapping to maintain grid alignment
      this.pre.style.whiteSpace = "pre";
      this.pre.style.overflow = "hidden";

      this.fontMeasured = true;
    }

    render(scene, camera) {
      this.renderer.render(scene, camera);

      const w = this.canvas.width;
      const h = this.canvas.height;
      this.context.clearRect(0, 0, w, h);
      if (this.context && w && h) {
        this.context.drawImage(this.renderer.domElement, 0, 0, w, h);
      }

      this.asciify(this.context, w, h);
      this.hue();
    }

    onMouseMove(e) {
      this.mouse = { x: e.clientX * PX_RATIO, y: e.clientY * PX_RATIO };
    }

    get dx() {
      return this.mouse.x - this.center.x;
    }

    get dy() {
      return this.mouse.y - this.center.y;
    }

    hue() {
      const deg = (Math.atan2(this.dy, this.dx) * 180) / Math.PI;
      this.deg += (deg - this.deg) * 0.075;
      this.domElement.style.filter = `hue-rotate(${this.deg.toFixed(1)}deg)`;
    }

    asciify(ctx, w, h) {
      if (w && h) {
        const imgData = ctx.getImageData(0, 0, w, h).data;
        let str = "";
        for (let y = 0; y < h; y++) {
          for (let x = 0; x < w; x++) {
            const i = x * 4 + y * 4 * w;
            const [r, g, b, a] = [
              imgData[i],
              imgData[i + 1],
              imgData[i + 2],
              imgData[i + 3],
            ];

            if (a === 0) {
              str += " ";
              continue;
            }

            let gray = (0.3 * r + 0.6 * g + 0.1 * b) / 255;
            let idx = Math.floor((1 - gray) * (this.charset.length - 1));
            if (this.invert) idx = this.charset.length - idx - 1;
            str += this.charset[idx];
          }
          str += "\n";
        }
        this.pre.textContent = str;
      }
    }

    dispose() {
      document.removeEventListener("mousemove", this.onMouseMove);
    }
  }

  class CanvasTxt {
    constructor(
      txt,
      {
        fontSize = 200,
        fontFamily = "Arial",
        color = "#fdf9f3",
        skewAngle = 0,
      } = {},
    ) {
      this.canvas = document.createElement("canvas");
      this.context = this.canvas.getContext("2d");
      this.txt = txt;
      this.fontSize = fontSize;
      this.fontFamily = fontFamily;
      this.color = color;
      this.skewAngle = skewAngle; // degrees, negative = italic/right slant
      this.font = `500 ${this.fontSize}px ${this.fontFamily}`;
      this.padLeft = 0;
      this.padRight = 2;
      this.padY = 10;
    }

    resize() {
      this.context.font = this.font;
      const metrics = this.context.measureText(this.txt);
      const textHeight =
        Math.ceil(
          metrics.actualBoundingBoxAscent + metrics.actualBoundingBoxDescent,
        ) +
        this.padY * 2;
      // Extra horizontal room to accommodate the skew
      const skewOffset =
        Math.abs(Math.tan((this.skewAngle * Math.PI) / 180)) * textHeight;
      const textWidth =
        Math.ceil(metrics.width) +
        this.padLeft +
        this.padRight +
        Math.ceil(skewOffset);
      this.canvas.width = textWidth;
      this.canvas.height = textHeight;
    }

    render() {
      this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.context.save();
      // Apply horizontal skew: [1, 0, tan(angle), 1, offsetX, 0]
      const skewX = Math.tan((this.skewAngle * Math.PI) / 180);
      // Shift right so skewed text doesn't clip on the left
      const offsetX = skewX < 0 ? -skewX * this.canvas.height : 0;
      this.context.transform(1, 0, skewX, 1, offsetX, 0);
      this.context.fillStyle = this.color;
      this.context.font = this.font;
      const metrics = this.context.measureText(this.txt);
      const yPos = this.padY + metrics.actualBoundingBoxAscent;
      this.context.fillText(this.txt, this.padLeft, yPos);
      this.context.restore();
    }

    get width() {
      return this.canvas.width;
    }
    get height() {
      return this.canvas.height;
    }
    get texture() {
      return this.canvas;
    }
  }

  class CanvAscii {
    constructor(
      {
        text,
        asciiFontSize,
        textFontSize,
        textColor,
        planeBaseHeight,
        enableWaves,
        skewAngle,
      },
      containerElem,
      width,
      height,
    ) {
      this.textString = text;
      this.asciiFontSize = asciiFontSize;
      this.textFontSize = textFontSize;
      this.textColor = textColor;
      this.planeBaseHeight = planeBaseHeight;
      this.skewAngle = skewAngle;
      this.container = containerElem;
      this.width = width;
      this.height = height;
      this.enableWaves = enableWaves;

      this.camera = new THREE.PerspectiveCamera(
        75,
        this.width / this.height,
        1,
        1000,
      );
      // Zoom camera closer to make text larger in smaller container
      this.camera.position.z = 10;
      this.scene = new THREE.Scene();
      this.mouse = { x: 0, y: 0 };
      this.mouseVelocity = 0;
      this.isDisposed = false;
      this.isAnimating = false;
      this.animationFrameId = 0;

      this.onMouseMove = this.onMouseMove.bind(this);
      this.handleVisibilityChange = this.handleVisibilityChange.bind(this);
      this.setMesh();
      this.setRenderer();
    }

    setMesh() {
      this.textCanvas = new CanvasTxt(this.textString, {
        fontSize: this.textFontSize,
        fontFamily: "Xenon",
        color: this.textColor,
        skewAngle: this.skewAngle,
      });
      this.textCanvas.resize();
      this.textCanvas.render();

      this.texture = new THREE.CanvasTexture(this.textCanvas.texture);
      this.texture.minFilter = THREE.NearestFilter;

      const textAspect = this.textCanvas.width / this.textCanvas.height;
      const baseH = this.planeBaseHeight;
      const planeW = baseH * textAspect;
      const planeH = baseH;

      this.geometry = new THREE.PlaneGeometry(planeW, planeH, 36, 36);
      this.material = new THREE.ShaderMaterial({
        vertexShader,
        fragmentShader,
        transparent: true,
        uniforms: {
          uTime: { value: 0 },
          mouse: { value: 1.0 },
          uTexture: { value: this.texture },
          uEnableWaves: { value: this.enableWaves ? 1.0 : 0.0 },
        },
      });

      this.mesh = new THREE.Mesh(this.geometry, this.material);
      this.alignMesh();
      this.scene.add(this.mesh);
    }

    setRenderer() {
      this.renderer = new THREE.WebGLRenderer({
        antialias: false,
        alpha: true,
      });
      this.renderer.setPixelRatio(1);
      this.renderer.setClearColor(0x000000, 0);

      this.filter = new AsciiFilter(this.renderer, {
        fontFamily: "Krypton",
        fontSize: this.asciiFontSize,
        invert: true,
      });

      this.container.appendChild(this.filter.domElement);
      this.setSize(this.width, this.height);

      // Listen on window instead of container (container has pointer-events-none)
      window.addEventListener("mousemove", this.onMouseMove);
      window.addEventListener("touchmove", this.onMouseMove);
    }

    setSize(w, h) {
      this.width = w;
      this.height = h;
      this.camera.aspect = w / h;
      this.camera.updateProjectionMatrix();
      this.filter.setSize(w, h);
      this.center = { x: w / 2, y: h / 2 };

      if (this.mesh && this.geometry) {
        this.alignMesh();
      }
    }

    alignMesh() {
      // Calculate visible area at z=0 using current camera FOV
      const fovRad = (this.camera.fov * Math.PI) / 180;
      const dist = this.camera.position.z;
      const visibleHeight = 2 * Math.tan(fovRad / 2) * dist;
      const visibleWidth = visibleHeight * this.camera.aspect;

      const meshWidth = this.geometry.parameters.width;
      const meshHeight = this.geometry.parameters.height;

      // Scale mesh to fill the width of the visible area
      const scale = (visibleWidth / meshWidth) * 0.95;
      this.mesh.scale.set(scale, scale, 1);

      // Center the mesh
      this.mesh.position.set(0, 0, 0);
    }

    load() {
      document.addEventListener(
        "visibilitychange",
        this.handleVisibilityChange,
      );
      this.startAnimation();
    }

    onMouseMove(evt) {
      const e = evt.touches ? evt.touches[0] : evt;
      const bounds = this.container.getBoundingClientRect();
      const x = e.clientX - bounds.left;
      const y = e.clientY - bounds.top;

      // Calculate velocity (acceleration)
      const dx = x - this.mouse.x;
      const dy = y - this.mouse.y;
      const velocity = Math.sqrt(dx * dx + dy * dy);
      this.mouseVelocity = Math.min(velocity / 10, 2); // Cap at 2x

      this.mouse = { x, y };
    }

    animateFrame() {
      if (this.isDisposed || !this.isAnimating) return;
      this.render();
      this.animationFrameId = requestAnimationFrame(() => this.animateFrame());
    }

    startAnimation() {
      if (this.isDisposed || this.isAnimating || document.hidden) return;
      this.isAnimating = true;
      this.animationFrameId = requestAnimationFrame(() => this.animateFrame());
    }

    stopAnimation() {
      this.isAnimating = false;
      if (this.animationFrameId) {
        cancelAnimationFrame(this.animationFrameId);
        this.animationFrameId = 0;
      }
    }

    handleVisibilityChange() {
      if (document.hidden) {
        this.stopAnimation();
      } else {
        this.startAnimation();
      }
    }

    render() {
      const time = new Date().getTime() * 0.001;
      this.textCanvas.render();
      this.texture.needsUpdate = true;
      this.mesh.material.uniforms.uTime.value = Math.sin(time);
      this.updateRotation();
      this.filter.render(this.scene, this.camera);
    }

    updateRotation() {
      // Decay mouse velocity over time
      this.mouseVelocity *= 0.95;

      // Update wave intensity based on mouse velocity
      const targetWave = 0.08 + this.mouseVelocity * 0.3;
      const currentWave = this.mesh.material.uniforms.uEnableWaves.value;
      this.mesh.material.uniforms.uEnableWaves.value +=
        (targetWave - currentWave) * 0.1;
    }

    clear() {
      this.scene.traverse((obj) => {
        if (
          obj.isMesh &&
          typeof obj.material === "object" &&
          obj.material !== null
        ) {
          Object.keys(obj.material).forEach((key) => {
            const matProp = obj.material[key];
            if (
              matProp !== null &&
              typeof matProp === "object" &&
              typeof matProp.dispose === "function"
            ) {
              matProp.dispose();
            }
          });
          obj.material.dispose();
          obj.geometry.dispose();
        }
      });
      this.scene.clear();
    }

    dispose() {
      if (this.isDisposed) return;
      this.isDisposed = true;
      this.stopAnimation();
      document.removeEventListener(
        "visibilitychange",
        this.handleVisibilityChange,
      );
      this.filter.dispose();
      if (this.container?.contains(this.filter.domElement)) {
        this.container.removeChild(this.filter.domElement);
      }
      window.removeEventListener("mousemove", this.onMouseMove);
      window.removeEventListener("touchmove", this.onMouseMove);
      this.clear();
      this.renderer.dispose();
    }
  }

  // Wait for fonts to load before initializing
  async function waitForFonts() {
    if (typeof document === "undefined") return;

    try {
      // Wait for the specific fonts we're using
      await document.fonts.load('600 12px "IBM Plex Mono"');
      await document.fonts.load(`600 ${textFontSize}px "Xenon"`);
      fontsLoaded = true;
    } catch (e) {
      console.warn("Font loading check failed, proceeding anyway:", e);
      fontsLoaded = true;
    }
  }

  onMount(() => {
    if (!container) return;

    let isActive = true;
    let fontDelayTimeout;

    (async () => {
      // Wait for fonts to be ready
      await waitForFonts();
      if (!isActive) return;

      // Small additional delay to ensure fonts are fully rendered
      await new Promise((resolve) => {
        fontDelayTimeout = setTimeout(resolve, 100);
      });
      if (!isActive) return;

      // Directly initialize with container dimensions
      asciiInstance = new CanvAscii(
        {
          text,
          asciiFontSize,
          textFontSize,
          textColor,
          planeBaseHeight,
          enableWaves,
          skewAngle,
        },
        container,
        containerWidth || 400,
        containerHeight || 200,
      );
      asciiInstance.load();
    })();

    return () => {
      isActive = false;
      clearTimeout(fontDelayTimeout);
      if (asciiInstance) {
        asciiInstance.dispose();
        asciiInstance = undefined;
      }
    };
  });
</script>

<div
  bind:this={container}
  bind:clientWidth={containerWidth}
  bind:clientHeight={containerHeight}
  class="ascii-text-container"
></div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@500;600&display=swap");

  .ascii-text-container {
    position: relative;
    overflow: hidden;
    width: 100%;
    height: 100%;
  }

  :global(.ascii-text-container canvas) {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    image-rendering: pixelated;
  }

  :global(.ascii-text-container pre) {
    margin: 0;
    user-select: none;
    padding: 0;
    line-height: 1em;
    text-align: left;
    position: absolute;
    left: 0;
    top: 0;
    background-image: radial-gradient(
      circle,
      #ff6188 0%,
      #fc9867 50%,
      #ffd866 100%
    );
    background-attachment: fixed;
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    background-clip: text;
    z-index: 9;
    /* Ensure consistent rendering */
    font-feature-settings: normal;
    font-variant: normal;
    text-rendering: geometricPrecision;
  }

  :global(.dark .ascii-text-container pre) {
    background-image: radial-gradient(
      circle,
      #ff6188 0%,
      #fc9867 50%,
      #ffd866 100%
    );
  }
</style>
