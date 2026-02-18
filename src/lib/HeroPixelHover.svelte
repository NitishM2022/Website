<script>
  import { onMount, onDestroy } from "svelte";

  export let gap = 5;
  export let speed = 35;
  export let colors = ["#f8fafc", "#f1f5f9", "#cbd5e1"];
  export let opacity = 0.55;
  export let alwaysOn = false;

  let hostEl;
  let canvasEl;
  let ctx;
  let pixels = [];
  let rafId = 0;
  let reducedMotion = false;
  let mode = "disappear";
  let isAnimating = false;
  let resizeObserver;
  let width = 1;
  let height = 1;

  class Pixel {
    constructor(x, y, color, speedScale, delay) {
      this.x = x;
      this.y = y;
      this.color = color;
      this.speed = speedScale;
      this.delay = delay;
      this.delayCount = 0;
      this.size = 0;
      this.maxSize = 1 + Math.random() * 1.4;
      this.growStep = 0.1 + Math.random() * 0.4;
      this.minSize = 0.4;
      this.reverse = false;
      this.shimmer = false;
      this.idle = true;
    }

    draw(ctx2d) {
      if (this.size <= 0) return;
      ctx2d.fillStyle = this.color;
      ctx2d.fillRect(this.x, this.y, this.size, this.size);
    }

    appear() {
      this.idle = false;
      if (this.delayCount < this.delay) {
        this.delayCount += 1.5;
        return;
      }

      if (this.size >= this.maxSize) this.shimmer = true;
      if (this.shimmer) {
        if (this.size >= this.maxSize) this.reverse = true;
        if (this.size <= this.minSize) this.reverse = false;
        this.size += this.reverse ? -this.speed : this.speed;
      } else {
        this.size += this.growStep;
      }
    }

    disappear() {
      this.shimmer = false;
      this.delayCount = 0;
      this.size = Math.max(0, this.size - 0.14);
      this.idle = this.size <= 0;
    }
  }

  function clampGap(value) {
    return Math.max(4, Math.min(30, Math.floor(value)));
  }

  function getSpeed(value) {
    if (reducedMotion) return 0;
    return Math.max(0, Math.min(100, value)) * 0.001;
  }

  function distanceToCenter(x, y) {
    const dx = x - width / 2;
    const dy = y - height / 2;
    return Math.sqrt(dx * dx + dy * dy) * 0.02;
  }

  function createPixels() {
    pixels = [];
    const step = clampGap(gap);
    const speedScale = getSpeed(speed);

    for (let x = 0; x < width; x += step) {
      for (let y = 0; y < height; y += step) {
        const color = colors[Math.floor(Math.random() * colors.length)];
        const delay = reducedMotion ? 0 : distanceToCenter(x, y);
        pixels.push(new Pixel(x, y, color, speedScale, delay));
      }
    }

    // Sort pixels by color to minimize state changes in render loop
    pixels.sort((a, b) => {
      if (a.color < b.color) return -1;
      if (a.color > b.color) return 1;
      return 0;
    });
  }

  function setupCanvas() {
    if (!canvasEl || !hostEl) return;
    const rect = hostEl.getBoundingClientRect();
    width = Math.max(1, Math.floor(rect.width));
    height = Math.max(1, Math.floor(rect.height));

    const dpr = window.devicePixelRatio || 1;
    canvasEl.width = Math.floor(width * dpr);
    canvasEl.height = Math.floor(height * dpr);
    canvasEl.style.width = `${width}px`;
    canvasEl.style.height = `${height}px`;

    ctx = canvasEl.getContext("2d");
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    createPixels();

    // Only start a new loop if one isn't already running
    if (!isAnimating) {
      renderFrame();
    }
  }

  function renderFrame() {
    if (!ctx) return;
    ctx.clearRect(0, 0, width, height);

    let allIdle = true;
    let lastColor = null;

    for (const pixel of pixels) {
      if (mode === "appear") pixel.appear();
      else pixel.disappear();

      // Only set fillStyle if color changes
      if (pixel.color !== lastColor) {
        ctx.fillStyle = pixel.color;
        lastColor = pixel.color;
      }

      // Draw pixel without setting style (Pixel.draw logic moved inline for performance)
      if (pixel.size > 0) {
        ctx.fillRect(pixel.x, pixel.y, pixel.size, pixel.size);
      }

      if (!pixel.idle) allIdle = false;
    }

    if (mode === "disappear" && allIdle) {
      isAnimating = false;
      cancelAnimationFrame(rafId);
      return;
    }

    rafId = requestAnimationFrame(renderFrame);
  }

  function start(modeName) {
    mode = modeName;
    if (isAnimating) return;
    isAnimating = true;
    rafId = requestAnimationFrame(renderFrame);
  }

  function onEnter() {
    start("appear");
  }

  function onLeave() {
    start("disappear");
  }

  onMount(() => {
    reducedMotion = window.matchMedia(
      "(prefers-reduced-motion: reduce)",
    ).matches;
    setupCanvas();

    resizeObserver = new ResizeObserver(() => setupCanvas());
    resizeObserver.observe(hostEl);

    const section = hostEl.parentElement;
    if (alwaysOn) {
      start("appear");
    } else if (section) {
      section.addEventListener("mouseenter", onEnter);
      section.addEventListener("mouseleave", onLeave);
      section.addEventListener("focusin", onEnter);
      section.addEventListener("focusout", onLeave);
    }
  });

  onDestroy(() => {
    cancelAnimationFrame(rafId);
    resizeObserver?.disconnect();
    const section = hostEl?.parentElement;
    if (!alwaysOn && section) {
      section.removeEventListener("mouseenter", onEnter);
      section.removeEventListener("mouseleave", onLeave);
      section.removeEventListener("focusin", onEnter);
      section.removeEventListener("focusout", onLeave);
    }
  });
</script>

<div
  bind:this={hostEl}
  class="pointer-events-none absolute inset-0"
  style={`opacity:${opacity};`}
  aria-hidden="true"
>
  <canvas bind:this={canvasEl} class="block h-full w-full"></canvas>
</div>
