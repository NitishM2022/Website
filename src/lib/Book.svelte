<script>
  export let name;
  export let author;
  export let image;
  export let length;
  export let link;
  export let color;

  const pageWidth = 30;
  let isHovered = false;
</script>

<div
  class="group relative flex flex-col items-center justify-end w-48 h-96"
  style="perspective: 1000px;"
  on:mouseenter={() => (isHovered = true)}
  on:mouseleave={() => (isHovered = false)}
  role="button"
  tabindex="0"
>
  <div
    class="book-wrapper relative w-52 h-80 transition-transform duration-500 ease-out"
    style="transform-style: preserve-3d; transform: rotateY({isHovered
      ? -15
      : -30}deg);"
  >
    <!-- Front Cover -->
    <div
      class="absolute inset-0 shadow-xl overflow-hidden"
      style="transform: translateZ({pageWidth}px); backface-visibility: hidden;"
    >
      <img src={image} alt={name} class="w-full h-full object-cover" />
      <div
        class="absolute inset-0 bg-gradient-to-r from-black/10 via-transparent to-transparent pointer-events-none"
      ></div>
    </div>

    <!-- Pages (right side edge) -->
    <div
      class="absolute inset-y-0 right-0"
      style="width: {pageWidth *
        2}px; transform: translateZ({pageWidth}px) rotateY(-90deg); transform-origin: right center; background: linear-gradient(to right, #f5f1e8, #e8e3d6);"
    >
      <div
        class="absolute inset-0"
        style="background-image: repeating-linear-gradient(0deg, rgba(0,0,0,0.03) 0px, rgba(0,0,0,0.03) 1px, transparent 1px, transparent 2px);"
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
    class="absolute -bottom-12 w-52 text-center transition-opacity duration-300 ease-out pointer-events-none"
    style="opacity: {isHovered ? 1 : 0};"
  >
    <h3 class="text-sm font-bold text-stone-900 dark:text-stone-100">{name}</h3>
    <p class="text-xs text-stone-600 dark:text-stone-400 mt-1">{author}</p>
  </div>
</div>
