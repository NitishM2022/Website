<script>
  export let name;
  export let description;
  export let link = {};
  export let date;
  export let tech = [];

  import Github from "./Github.svelte";
  import cpu from "./assets/cpu.png";
</script>

<div class="flex flex-col rounded-lg">
  <div
    class="h-50 sm:h-auto overflow-hidden rounded-t-lg bg-neutral-200 dark:bg-neutral-800"
  >
    <img
      src={cpu}
      alt="Nitish"
      class="w-4/5 sm:w-66 object-cover rounded-lg transition-transform duration-300 transform hover:scale-105 sm:-rotate-4 sm:translate-x-8.5 translate-y-8 shadow dark:shadow-white block mx-auto sm:mx-0"
    />
  </div>
  <div
    class="h-auto sm:h-66 p-3 border-b-1 border-l-1 border-r-1 border-neutral-200 dark:border-neutral-800 rounded-b-lg flex flex-col"
  >
    <h3 class="text-xl font-medium text-stone-950 dark:text-stone-300">
      {name}
    </h3>

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

    <p class="text-sm text-stone-500 dark:text-stone-400 mb-8 sm:mb-0">
      {description}
    </p>

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
</div>
