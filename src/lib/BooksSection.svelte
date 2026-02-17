<script>
  import { onMount } from "svelte";
  import Book from "./Book.svelte";
  import books from "./assets/books.json";

  let maxBooks = books.length;
  let booksContainer;

  // Calculate how many books can fit based on container width
  function calculateMaxBooks(containerWidth) {
    if (containerWidth <= 0) return 4;
    const firstBookWidth = 140;
    const additionalBookWidth = 100; // 140px - 40px overlap (-ml-10)
    const padding = 50;

    const availableWidth = containerWidth - padding;
    if (availableWidth < firstBookWidth) return 1;

    const additionalBooks = Math.floor(
      (availableWidth - firstBookWidth) / additionalBookWidth,
    );
    return Math.min(1 + additionalBooks, books.length);
  }

  $: displayedBooks = books.slice(0, maxBooks);

  onMount(() => {
    const updateBookCount = () => {
      if (booksContainer) {
        const width = booksContainer.getBoundingClientRect().width;
        maxBooks = calculateMaxBooks(width);
      }
    };

    setTimeout(updateBookCount, 100);

    window.addEventListener("resize", updateBookCount);
    return () => window.removeEventListener("resize", updateBookCount);
  });
</script>

<section class="mt-5">
  <h2
    class=" mb-5 px-4 text-2xl leading-10 font-medium text-stone-950 dark:text-stone-100"
  >
    Reading
  </h2>
  <div
    bind:this={booksContainer}
    class="grid-f min-[840px]:grid-f-lr flex h-80 flex-row items-end gap-0 overflow-x-visible overflow-y-visible font-light"
  >
    {#each displayedBooks as { name, image, length }, index}
      <div
        class="-ml-10 flex-shrink-0 transition-all duration-500 ease-out first:ml-0 hover:z-50"
        style="z-index: {100 - index};"
      >
        <Book {name} {image} {length} {index} />
      </div>
    {/each}
  </div>
</section>
