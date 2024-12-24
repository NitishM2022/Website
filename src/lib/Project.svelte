<script>
  export let name;
  export let description;
  export let link = {};
  export let date;
  export let tech = [];

  import Github from "./Github.svelte";
  import cpu from "./assets/cpu.png";
  console.log(link.github);
</script>

<div class="flex flex-col rounded-lg">
  <div
    class="tile relative overflow-hidden rounded-t-lg bg-stone-100 border-1 border-neutral-200 dark:bg-neutral-800 dark:border-neutral-700"
  >
    <img
      src={cpu}
      alt="Nitish"
      class="h-50 object-cover rounded-lg transition-transform duration-300 transform hover:scale-110 -rotate-z-4 translate-x-9 translate-y-8 shadow dark:shadow-white"
    />
  </div>
  <div
    class="h-66 p-3 border-b-1 border-l-1 border-r-1 border-neutral-200 dark:border-neutral-700 rounded-b-lg flex flex-col"
  >
    <h3 class="text-xl font-medium text-stone-950 dark:text-stone-300">
      {name}
    </h3>

    <p
      class="text-xs text-thin text-stone-500 dark:text-stone-400 font-mono mb-1.5"
    >
      {date}
    </p>

    <div class="flex flex-row gap-1 items-center mb-1.5">
      {#each tech as tech}
        <p
          class="border-1 px-2 rounded-2xl text-sm text-stone-950 dark:text-stone-300 dark:border-stone-100"
        >
          {tech}
        </p>
      {/each}
    </div>

    <p class="text-sm text-stone-500 dark:text-stone-400">
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

<style>
  .arrow {
    font-family:
      JetBrains Mono,
      sans-serif;
    font-size: 1.3rem;
    line-height: 1;
  }
</style>
