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

<div>
  <div
    class="flex flex-row items-center justify-between space-x-4 border-t-1 dark:border-stone-100 pt-2 mb-2"
  >
    <div class="flex flex-row gap-2">
      <h3 class="text-xl font-medium text-stone-950 dark:text-stone-300">
        {name}
      </h3>
    </div>

    <p
      class="text-sm text-thin text-stone-500 dark:text-stone-400 font-mono italic"
    >
      {date}
    </p>
  </div>
  <div class="flex flex-row justify-between">
    <div class="flex flex-col gap-2 w-100">
      <div class="flex flex-row gap-1 items-">
        {#each tech as tech}
          <p
            class="border-1 px-2 rounded-2xl text-sm text-stone-950 dark:text-stone-300 dark:border-stone-100"
          >
            {tech}
          </p>
        {/each}
      </div>
      <p class="text-sm text-stone-500 dark:text-stone-400 mb-2">
        {description}
      </p>
      <div class="flex-grow"></div>
      {#if link.github}
        <button
          style="width: fit-content;"
          class="hover:opacity-70 hover:cursor-pointer flex flex-row items-center gap-1.5 bg-stone-950 dark:bg-stone-100 text-stone-100 dark:text-stone-950 px-1.5 py-0.5 rounded-lg"
          on:click={window.open(link.github, "_blank")}
        >
          <Github /> Github
        </button>
      {/if}
    </div>
    <div class="tile relative overflow-hidden rounded-lg bg-amber-50">
      <img
        src={cpu}
        alt="Nitish"
        class="w-70 h-50 object-cover rounded-lg transition-transform duration-300 transform hover:scale-110 -rotate-z-6 translate-x-10 translate-y-8 drop-shadow-2xl"
      />
    </div>
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
