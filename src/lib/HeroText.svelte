<script>
    import { onMount, onDestroy } from "svelte";
    import DecryptedText from "./DecryptedText.svelte";

    const bioTexts = [
        "studying Computer Science at Texas A&M",
        "who engineered a cross-platform C++ visualization tool at Amazon",
        "who developed an AI-powered BIOS debugger using Llama 3 at Dell",
        "Eagle Scout, President's Scholar, and USACO Gold member",
    ];

    let { maxLength = 64 } = $props();
    let currentIndex = $state(0);
    let interval;

    onMount(() => {
        interval = setInterval(() => {
            currentIndex = (currentIndex + 1) % bioTexts.length;
        }, 4000);
    });

    onDestroy(() => {
        if (interval) clearInterval(interval);
    });
</script>

<div
    class="uppercase tracking-tight leading-10 text-2xl sm:text-[25.5px] text-stone-950 dark:text-stone-300 h-40 sm:min-h-[120px] sm:h-auto overflow-hidden"
    style="font-family: 'Geist Mono', monospace;"
>
    I'm Nitish, a developing <a
        href="https://paulgraham.com/hp.html"
        target="_blank"
        class="underline underline-offset-4 decoration-2 link">builder</a
    >,
    <DecryptedText
        text={bioTexts[currentIndex]}
        animateOn="view"
        revealDirection="center"
        speed={60}
        {maxLength}
    />
</div>
