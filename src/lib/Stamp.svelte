<script>
  import Mark from "./Mark.svelte";
</script>

<div class="ml-1.5 flex flex-row relative">
  <div class="stamp-container -rotate-2 relative z-0">
    <div class="stamp mb-5">
      <div
        class="m-1 rounded-sm p-2 px-3 bg-cover bg-center bg-[url('./lib/assets/stamp.jpeg')]"
      >
        <h1
          class="text-stone-100 text-5xl font-medium font-serif italic z-20 relative"
        >
          Howdy
        </h1>
      </div>
    </div>
  </div>
  <div class="w-36 -rotate-2 -translate-x-9 -translate-y-3 z-10">
    <Mark />
  </div>
</div>

<style>
  .stamp {
    --r: 4px;

    display: inline-block; /* Makes it fit to content */
    padding: calc(2 * var(--r));
    background: rgb(243, 243, 243);
    mask:
      radial-gradient(var(--r), #0000 97%, #000) round calc(-1.5 * var(--r))
        calc(-1.5 * var(--r)) / calc(3 * var(--r)) calc(3 * var(--r)),
      linear-gradient(#000 0 0) no-repeat 50% / calc(100% - 3 * var(--r))
        calc(100% - 3 * var(--r));
    width: fit-content; /* Makes it fit to content width */
  }

  .stamp-container {
    display: inline-block;
    filter: drop-shadow(3px 3px 4px rgba(0, 0, 0, 0.2));
  }

  :global(.dark) .stamp-container {
    filter: drop-shadow(3px 3px 4px rgba(200, 200, 200, 0.3));
  }
</style>
