<script>
  export let name;
  export let description;
  export let link = {};
  export let date;
  export let tech = [];
  export let images = [];

  import Github from "./Github.svelte";
  import Link from "./Link.svelte";
</script>

<div
  class="sm:h-100 flex flex-col bg-stone-100 dark:bg-neutral-800 border-1 border-neutral-200 dark:border-neutral-700 overflow-hidden shadow-lg dark:shadow-neutral-800"
>
  <div class="px-6 pt-4 flex flex-col mb-6">
    <div class="flex flex-row items-center">
      <h3 class="text-xl font-medium text-stone-950 dark:text-stone-300">
        {name}
      </h3>
      <div class="grow"></div>

      <div class="flex flex-row gap-3">
        {#if link.apple}
          <Link href={link.apple} name="APPLE">
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
          <Link href={link.github} name="GITHUB">
            <div class=" text-stone-950 dark:text-stone-100 items-center">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="w-4 h-4"
                aria-hidden="true"
                ><path
                  d="M15 22v-4a4.8 4.8 0 0 0-1-3.5c3 0 6-2 6-5.5.08-1.25-.27-2.48-1-3.5.28-1.15.28-2.35 0-3.5 0 0-1 0-3 1.5-2.64-.5-5.36-.5-8 0C6 2 5 2 5 2c-.3 1.15-.3 2.35 0 3.5A5.403 5.403 0 0 0 4 9c0 3.5 3 5.5 6 5.5-.39.49-.68 1.05-.85 1.65-.17.6-.22 1.23-.15 1.85v4"
                ></path><path d="M9 18c-4.51 2-5-2-7-2"></path></svg
              >
            </div>
          </Link>
        {/if}
      </div>
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
  {#if images.length == 2}
    <div class="flex flex-row gap-2 px-7 xsm:px-10">
      <img
        src={images[0]}
        alt="Nitish"
        class="hover:z-10 double drop-shadow-sm xsm:w-7/12 sm:h-64 object-cover object-left-top xsm:-rotate-3 xsm:-translate-x-1 translate-y-5 rounded-lg transition duration-300 ease-in-out hover:scale-102 border-4 border-white"
      />
      <img
        src={images[1]}
        alt="Nitish"
        class="hover:z-10 hidden xsm:inline double drop-shadow-sm w-5/12 sm:h-64 object-cover object-left-top rotate-2 -translate-x-1 translate-y-4 rounded-lg transition duration-300 ease-in-out hover:scale-102"
      />
    </div>
  {:else if images.length == 1}
    <div class="flex flex-row gap-2 px-7">
      <img
        src={images[0]}
        alt="Nitish"
        class="mx-auto double drop-shadow-sm object-cover object-left-top translate-y-5 rounded-lg transition duration-300 ease-in-out hover:scale-102 border-4 border-white"
      />
    </div>
  {/if}
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

  :global(.dark) .double {
    border: 0px solid #eaeaea;
    outline: 4.5px solid rgb(60, 60, 60);
  }
</style>
