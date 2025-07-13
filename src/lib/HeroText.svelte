<script>
    import { onMount } from "svelte";
    import TextScramble from "./TextScramble.svelte";

    const bioTexts = [
        'studying CS with a minor in Embedded Systems at Texas A&M',
        'interested in Graphics, Comp Arch, and ML',
        'who worked at Dell where I developed a BIOS debugger using GenAI and RAG',
        'Eagle Scout, President\'s Scholar, and USACO Gold division member',
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

