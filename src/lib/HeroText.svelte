<script>
    import { onMount } from "svelte";
    import TextScramble from "./TextScramble.svelte";

    // Bio texts for cycling example
    const bioTexts = [
        'studying <span class="emph">CS</span> with a minor in <span class="emph">Embedded Systems</span> at <span class="tamu">Texas A&M</span>',
        'interested in <span class="emph">Graphics,</span> <span class="emph">Computer Architecture,</span> and <span class="emph">ML</span>',
        'who worked at <span class="emph">Dell</span> where I developed a <span class="emph">BIOS</span> debugger using <span class="emph">GenAI</span> and <span class="emph">RAG</span>',
        '<span class="emph">Eagle Scout,</span> <span class="emph">President\'s Scholar,</span> and <span class="gold">USACO Gold</span> division member',
    ];

    // Simple demo texts
    const hoverText = "Hover over me to scramble text!";
    const clickText = "Click me for a scramble effect!";

    // Component references
    let bioScrambler;
    let clickScrambler;
    let hoverScrambler;
    let manualText = $state("Try the manual scramble button!");
    let manualScrambler;

    function handleClickScramble() {
        clickScrambler.setText(clickText);
    }

    function handleHoverStart() {
        hoverScrambler.setText(hoverText);
    }

    function scrambleManualText() {
        manualScrambler.setText(manualText);
    }

    function changeManualText() {
        manualText = "Text changed! Now scramble me again.";
        // We don't call setText here - wait for the button click
    }

    onMount(() => {
        // You can programmatically control the scrambler if needed
        // For example, pause/restart the bio text cycle
        // Example: setTimeout(() => bioScrambler.stopCycle(), 10000);
    });
</script>

<div
    class="text-2xl xs:leading-10 font-rope xs:text-[30px] text-stone-950 dark:text-stone-300"
>
    <p>
        I'm Nitish, an aspiring <a
            href="https://paulgraham.com/hp.html"
            target="_blank"
            class="underline underline-offset-4 decoration-2">builder</a
        >,
    </p>
    <TextScramble
        bind:this={bioScrambler}
        textArray={bioTexts}
        cycleInterval={4000}
    />
</div>

<style>
    :global(.emph) {
        color: #78716c;
    }

    :global(.tamu) {
        color: #750000;
        background-image: linear-gradient(315deg, #750000 21%, #c14444 81%);
        background-clip: text;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    :global(.gold) {
        background:
            radial-gradient(
                ellipse farthest-corner at right bottom,
                #fedb37 0%,
                #fdb931 8%,
                #9f7928 30%,
                #8a6e2f 40%,
                transparent 130%
            ),
            radial-gradient(
                ellipse farthest-corner at left top,
                #ffffff 0%,
                #ffffac 8%,
                #d1b464 25%,
                #5d4a1f 62.5%,
                #5d4a1f 100%
            );
        -webkit-background-clip: text;
        background-clip: text;
        color: transparent;
    }
</style>
