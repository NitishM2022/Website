<script>
  export let name;
  export let image;
  export let length;
  export let index = 0;

  // Calculate page thickness based on book length (simple: pages / 20, clamped 15-30px)
  $: pageWidth = Math.min(30, Math.max(15, Math.round(parseInt(length) / 20)));

  const bookWidth = 140; // Fixed width in pixels
  let isHovered = false;
</script>

<div
  class="group relative flex flex-col items-center justify-end"
  style="perspective: 800px; width: {bookWidth}px; transform: translate(20px, -60px);"
  on:mouseenter={() => (isHovered = true)}
  on:mouseleave={() => (isHovered = false)}
  role="button"
  tabindex="0"
>
  <div
    class="book-wrapper relative transition-transform duration-500 ease-out"
    style="transform-style: preserve-3d; width: {bookWidth}px; transform: rotateY(-30deg) {isHovered &&
    index > 0
      ? 'translateX(50px)'
      : ''};"
  >
    <!-- Front Cover - this determines the height -->
    <div
      class="relative overflow-hidden shadow-xl"
      style="transform: translateZ({pageWidth}px); backface-visibility: hidden;"
    >
      <img src={image} alt={name} class="block h-auto w-full" />
      <div
        class="pointer-events-none absolute inset-0 bg-gradient-to-r from-black/10 via-transparent to-transparent"
      ></div>
    </div>

    <!-- Pages (right side edge) - positioned absolutely to match front cover height -->
    <div
      class="absolute top-1 right-1 bottom-1"
      style="width: {pageWidth * 2 - 4}px; transform: translateZ({pageWidth -
        2}px) rotateY(-90deg); transform-origin: right center; background: linear-gradient(to right, #f5f1e8, #e8e3d6);"
    >
      <div
        class="absolute inset-0"
        style="background-image: repeating-linear-gradient(90deg, rgba(0,0,0,0.03) 0px, rgba(0,0,0,0.03) 2px, transparent 2px, transparent 4px);"
      ></div>
    </div>

    <!-- Back Cover -->
    <div
      class="absolute inset-0 shadow-xl"
      style="transform: translateZ(-{pageWidth}px); backface-visibility: hidden;"
    >
      <div
        class="absolute inset-0 bg-gradient-to-br from-neutral-700 to-neutral-900"
      ></div>
    </div>

    <!-- Inner Back Cover (black) -->
    <div
      class="absolute inset-0 bg-black"
      style="transform: translateZ(-{pageWidth}px) rotateY(180deg); backface-visibility: hidden;"
    ></div>
  </div>

  <!-- Book info on hover -->
  <div
    class="pointer-events-none absolute -bottom-6 left-0 w-full px-1 text-center transition-all duration-500 ease-out"
    style="opacity: {isHovered
      ? 1
      : 0}; transform: translateZ(10px) rotateY(-30deg) {isHovered && index > 0
      ? 'translateX(45px)'
      : ''}; transform-origin: left center; transform-style: preserve-3d;"
  >
    <h3 class="truncate text-xs font-bold text-stone-900 dark:text-stone-100">
      {name}
    </h3>
  </div>
</div>
