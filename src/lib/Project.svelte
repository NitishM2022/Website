<script>
  export let name;
  export let description;
  export let link = {};
  export let date;
  export let tech = [];

  import Github from "./Github.svelte";
  import cpu from "./assets/cpu.png";
</script>

<div
  class="sm:h-100 flex flex-col bg-stone-100 dark:bg-neutral-800 border-1 border-neutral-200 dark:border-neutral-700 overflow-hidden shadow-lg dark:shadow-neutral-800"
>
  <div class="px-6 pt-5 flex flex-col mb-8">
    <div class="flex flex-row items-center">
      <h3 class="text-xl font-medium text-stone-950 dark:text-stone-300">
        {name}
      </h3>
      <div class="grow"></div>

      {#if link.github}
        <button
          style="width: fit-content;"
          class="hover:opacity-70 hover:cursor-pointer flex flex-row items-center gap-1.5 bg-stone-950 dark:bg-stone-100 text-stone-100 dark:text-stone-950 px-1.5 py-0.5 rounded-md"
          on:click={window.open(link.github, "_blank")}
        >
          <Github /> Github
        </button>
      {/if}
    </div>

    <p
      class="text-xs text-thin text-stone-500 dark:text-stone-400 font-mono mb-1.5"
    >
      {date}
    </p>

    <div class="flex flex-wrap gap-1 items-center mb-1.5">
      {#each tech as tech}
        <p
          class="border-1 px-2 rounded-2xl text-sm text-stone-950 dark:text-stone-300 dark:border-stone-100"
        >
          {tech}
        </p>
      {/each}
    </div>

    <p class="text-sm text-stone-500 dark:text-stone-400 mb-0 sm:mb-0">
      {description}
    </p>
  </div>
  <div class="flex flex-row gap-2 px-10">
    <img
      src={cpu}
      alt="Nitish"
      class="z-10 double drop-shadow-sm dark:shadow-lg dark:shadow-neutral-700 w-7/12 object-cover -rotate-3 -translate-x-1 translate-y-5 rounded-lg transition duration-300 ease-in-out hover:scale-102 border-4 border-white"
    />
    <img
      src={cpu}
      alt="Nitish"
      class="double drop-shadow-sm dark:shadow-lg dark:shadow-neutral-700 w-5/12 object-cover rotate-2 -translate-x-1 translate-y-4 rounded-lg transition duration-300 ease-in-out hover:scale-102"
    />
  </div>
</div>

<style>
  .gradient {
    background-image: none; /* Initially no gradient */
    transition: background-image 300ms ease-in-out; /* Transition for background-image */
  }

  .gradient:hover {
    background-image: radial-gradient(
      ellipse 60% 80% at 100% 0%,
      rgb(255, 234, 0) 0%,
      hsla(58, 100%, 50%, 0.6) 20%,
      hsla(58, 100%, 50%, 0.3) 40%,
      hsla(58, 100%, 50%, 0.1) 60%,
      transparent 80%
    );
  }

  .dark .gradient {
    background-image: radial-gradient(
      ellipse 100% 35% at 80%,
      hsla(239, 100%, 50%, 0.9) 0%,
      hsla(239, 100%, 50%, 0.6) 20%,
      hsla(239, 100%, 50%, 0.3) 40%,
      hsla(239, 100%, 50%, 0.1) 60%,
      transparent 80%
    );
  }

  .double {
    border: 0.5px solid #eaeaea;
    outline: 4px solid #ffffff;
  }
</style>
