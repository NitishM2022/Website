<script>
  import Mark from "./Mark.svelte";
</script>

<div class="relative ml-1.5 flex flex-row">
  <div class="stamp-container relative z-0 -rotate-2">
    <div class="stamp mb-5">
      <div
        class="m-1 rounded-sm bg-[url('./lib/assets/stamp.jpeg')] bg-cover bg-center p-2 px-3"
      >
        <h1
          class="relative z-20 font-serif text-5xl font-medium text-stone-100 italic"
        >
          Howdy
        </h1>
      </div>
    </div>
  </div>
  <div class="z-10 w-36 -translate-x-9 -translate-y-3 -rotate-2">
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
