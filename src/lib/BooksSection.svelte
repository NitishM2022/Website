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

<section class="fade-in-bottom-third mt-15">
    <h2
        class=" font-medium text-2xl leading-10 text-stone-950 dark:text-stone-100 mb-5 px-2 sm:px-0"
    >
        Reading
    </h2>
    <div
        bind:this={booksContainer}
        class="grid-border font-light flex flex-row gap-0 overflow-x-visible overflow-y-visible items-end h-80"
    >
        {#each displayedBooks as { name, image, length }, index}
            <div
                class="flex-shrink-0 transition-all duration-500 ease-out hover:z-50 -ml-10 first:ml-0"
                style="z-index: {100 - index};"
            >
                <Book {name} {image} {length} {index} />
            </div>
        {/each}
    </div>
</section>

<style>
    @keyframes fadeInBottom {
        from {
            opacity: 0;
            transform: translateY(60px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    .fade-in-bottom-third {
        animation: fadeInBottom 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        animation-fill-mode: both;
        animation-delay: 0.2s;
    }
</style>
