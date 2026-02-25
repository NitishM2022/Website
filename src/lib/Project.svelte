<script>
  export let name;
  export let description;
  export let link = {};
  export let tech = [];
  export let images = [];

  import Link from "./Link.svelte";
</script>

<div class="grid-tb min-[840px]:grid-tb-lr flex flex-col pb-[1px]">
  <div class="mb-10 flex h-44 flex-col overflow-hidden px-5 pt-4 sm:h-34">
    <div class="flex items-center">
      <h3 class="line text-xl font-medium text-stone-950 dark:text-stone-300">
        {name}
      </h3>
      <div class="grow"></div>

      <div class="flex gap-3">
        {#if link.apple}
          <Link href={link.apple}>
            <div class=" items-center text-stone-950 dark:text-stone-100">
              <i class="ri-apple-line align-middle text-xl"></i>
            </div>
          </Link>
        {/if}
        {#if link.github}
          <Link href={link.github}>
            <div class="items-center text-stone-950 dark:text-stone-100">
              <i class="ri-github-line align-middle text-xl" aria-hidden="true"
              ></i>
            </div>
          </Link>
        {/if}
      </div>
    </div>

    <div class="mt-1.5 mb-2 flex flex-wrap items-center gap-1">
      {#each tech as tech}
        <p
          class="rounded-2xl bg-stone-800 px-2 text-sm text-stone-100 dark:bg-stone-200 dark:text-stone-900"
        >
          {tech}
        </p>
      {/each}
    </div>

    <p class="mb-0 text-sm text-stone-500 min-[840px]:mb-0 dark:text-stone-400">
      {description}
    </p>
  </div>
  <div class="flex h-[190px] flex-col overflow-hidden px-[1px] sm:h-[260px]">
    {#if images.length == 2}
      <div class="mt-auto flex w-full gap-[1px]">
        <img
          src={images[0]}
          class="max-h-72 w-4/7 object-cover object-top-left"
        />
        <img
          src={images[1]}
          class="max-h-72 w-3/7 object-cover object-top-left"
        />
      </div>
    {:else if images.length == 1}
      <img
        src={images[0]}
        class="mt-auto max-h-72 w-full object-cover object-top"
      />
    {/if}
  </div>
</div>
