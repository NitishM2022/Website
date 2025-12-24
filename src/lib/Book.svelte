<script>
  export let name;
  export let author;
  export let image;
  export let length;
  export let link;
  export let color = "#1f2937";

  // Dynamic thickness based on length (pages) or a default
  $: thickness = 50;
</script>

<div
  class="group relative h-96 flex flex-col justify-end items-center transition-all duration-500 ease-out w-32 hover:w-56 perspective-midrange"
  style="--thickness: {thickness}px; --spine-color: {color};"
>
  <div
    class="relative w-full h-80 cursor-pointer"
    style="transform-style: preserve-3d;"
  >
    <div
      class="absolute left-0 bottom-0 h-full origin-right transition-all duration-500 ease-out text-white rounded-l-sm z-20"
      style="width: var(--thickness); background-color: var(--spine-color); backface-visibility: hidden; transform: rotateY(-45deg) translateZ(0px);"
    >
      <div
        class="w-full h-full flex items-center justify-center overflow-hidden relative"
      >
        <span class="absolute inset-0 opacity-20 pointer-events-none z-10"
        ></span>

        <h2
          class="m-0 text-[10px] font-black tracking-widest uppercase whitespace-nowrap"
          style="writing-mode: vertical-rl; text-orientation: mixed;"
        >
          {name}
        </h2>
      </div>
    </div>

    <div
      class="absolute bottom-0 w-52 h-full origin-left transition-all duration-500 ease-out bg-stone-200 z-10 shadow-md group-hover:shadow-2xl"
      style="left: var(--thickness); backface-visibility: hidden; transform: rotateY(45deg);"
    >
      <div class="w-full h-full relative overflow-hidden">
        <span class="absolute inset-0 opacity-30 pointer-events-none z-10"
        ></span>
        <span
          class="absolute inset-0 pointer-events-none z-20 bg-gradient-to-br from-white/10 to-black/5"
        ></span>
        <img src={image} alt={name} class="w-full h-full object-cover block" />
      </div>
    </div>
  </div>

  <div
    class="translate-x-7 absolute -bottom-14 w-52 text-center transition-opacity duration-300 ease-out opacity-0 pointer-events-none z-50 group-hover:opacity-100"
  >
    <h3 class="text-sm font-bold text-stone-900 dark:text-stone-100">{name}</h3>
    <p class="text-xs text-stone-600 dark:text-stone-400 mt-0.5">{author}</p>
  </div>
</div>

<style>
  .group:hover .absolute.left-0 {
    transform: rotateY(-70deg) translateZ(0px) !important;
  }

  .group:hover .absolute.bottom-0:not(.left-0) {
    transform: rotateY(20deg) translateZ(0px) !important;
  }
</style>
