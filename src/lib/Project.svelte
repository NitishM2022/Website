<script>
  export let name;
  export let description;
  export let link = {};
  export let tech = [];
  export let images = [];

  import GithubIcon from "./GithubIcon.svelte";
  import Link from "./Link.svelte";
</script>

<div
  class="flex flex-col bg-[#fafaf9] dark:bg-[#171717] grid-border overflow-hidden"
>
  <div class="flex flex-col px-5 pt-4 sm:h-40 h-50">
    <div class="flex items-center">
      <h3 class="text-xl font-medium text-stone-950 dark:text-stone-300 line">
        {name}
      </h3>
      <div class="grow"></div>

      <div class="flex gap-3">
        {#if link.apple}
          <Link href={link.apple}>
            <div class=" text-stone-950 dark:text-stone-100 items-center">
              <svg
                width="24"
                height="24"
                viewBox="0 0 583 515"
                fill="currentColor"
                stroke="currentColor"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="w-4 h-4"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M290.6 46.8L306.8 18.8C316.8 1.30001 339.1 -4.6 356.6 5.4C374.1 15.4 380 37.7 370 55.2L213.9 325.4H326.8C363.4 325.4 383.9 368.4 368 398.2H37C16.8 398.2 0.600006 382 0.600006 361.8C0.600006 341.6 16.8 325.4 37 325.4H129.8L248.6 119.5L211.5 55.1C201.5 37.6 207.4 15.5 224.9 5.30001C242.4 -4.69999 264.5 1.2 274.7 18.7L290.6 46.8ZM150.2 435.7L115.2 496.4C105.2 513.9 82.9 519.8 65.4 509.8C47.9 499.8 42 477.5 52 460L78 415C107.4 405.9 131.3 412.9 150.2 435.7ZM451.6 325.6H546.3C566.5 325.6 582.7 341.8 582.7 362C582.7 382.2 566.5 398.4 546.3 398.4H493.7L529.2 460C539.2 477.5 533.3 499.6 515.8 509.8C498.3 519.8 476.2 513.9 466 496.4C406.2 392.7 361.3 315.1 331.5 263.4C301 210.8 322.8 158 344.3 140.1C368.2 181.1 403.9 243 451.6 325.6Z"
                />
              </svg>
            </div>
          </Link>
        {/if}
        {#if link.github}
          <Link href={link.github}>
            <div class="text-stone-950 dark:text-stone-100 items-center">
              <GithubIcon />
            </div>
          </Link>
        {/if}
      </div>
    </div>

    <div class="flex flex-wrap gap-1 items-center mt-1.5 mb-2">
      {#each tech as tech}
        <p
          class="px-2 rounded-2xl text-sm bg-stone-800 text-stone-100 dark:bg-stone-200 dark:text-stone-900"
        >
          {tech}
        </p>
      {/each}
    </div>

    <p class="text-sm text-stone-500 dark:text-stone-400 mb-0 min-[840px]:mb-0">
      {description}
    </p>
  </div>
  <div class="min-h-50 flex items-end">
    {#if images.length == 2}
      <div class="flex grid-border w-full gap-[1px]">
        <img
          src={images[0]}
          class="w-4/7 max-h-60 object-cover object-top-left"
        />
        <img src={images[1]} class="w-3/7 max-h-60 object-cover object-top" />
      </div>
    {:else if images.length == 1}
      <img src={images[0]} class="w-full max-h-60 object-cover object-top" />
    {/if}
  </div>
</div>
