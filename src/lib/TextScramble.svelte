<script>
    import { onMount, onDestroy } from "svelte";

    // Props
    const {
        text,
        textArray = [],
        cycleInterval = 4000,
        scrambleDuration = 40,
        autoStart = true,
        preserveHtml = true,
        scrambleOnMount = true,
    } = $props();

    // State
    let currentIndex = $state(0);
    let element; // Reference to the DOM element
    let cycleIntervalId = null;
    let frameRequest = null;
    let isAnimating = $state(false);

    // Scramble configuration
    const chars =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+-=[]{}|;:',./<>?";

    // Methods
    function parseHtml(html) {
        // Create a temp element to handle HTML properly
        const tempDiv = document.createElement("div");
        tempDiv.innerHTML = html || "";
        return tempDiv.textContent || "";
    }

    async function setText(newText) {
        if (!element || isAnimating) return;

        isAnimating = true;

        // Get the text content
        const oldText = preserveHtml
            ? parseHtml(element.innerHTML)
            : element.textContent || "";
        const plainNewText = preserveHtml ? parseHtml(newText) : newText;

        // Store final HTML for replacement when animation completes
        const finalContent = preserveHtml ? newText : plainNewText;

        // Create a queue of character transitions
        const length = Math.max(oldText.length, plainNewText.length);
        const queue = [];

        for (let i = 0; i < length; i++) {
            const from = oldText[i] || "";
            const to = plainNewText[i] || "";
            const start = Math.floor(Math.random() * scrambleDuration);
            const end = start + Math.floor(Math.random() * scrambleDuration);
            queue.push({ from, to, start, end, char: null });
        }

        // Start the animation
        let frame = 0;

        return new Promise((resolve) => {
            function update() {
                let output = "";
                let complete = 0;

                for (let i = 0; i < queue.length; i++) {
                    let { from, to, start, end, char } = queue[i];

                    if (frame >= end) {
                        complete++;
                        output += to;
                    } else if (frame >= start) {
                        if (!char || Math.random() < 0.28) {
                            char =
                                chars[Math.floor(Math.random() * chars.length)];
                            queue[i].char = char;
                        }
                        output += `<span class="scramble-char">${char}</span>`;
                    } else {
                        output += from;
                    }
                }

                element.innerHTML = output;

                if (complete === queue.length) {
                    // Animation complete
                    element.innerHTML = finalContent;
                    isAnimating = false;
                    resolve();
                } else {
                    frame++;
                    frameRequest = requestAnimationFrame(update);
                }
            }

            // Cancel any existing animation
            if (frameRequest) {
                cancelAnimationFrame(frameRequest);
            }

            update();
        });
    }

    async function cycleTo(index) {
        if (textArray.length === 0) return;

        currentIndex = index % textArray.length;
        await setText(textArray[currentIndex]);
    }

    async function cycleNext() {
        await cycleTo(currentIndex + 1);
    }

    function startCycle() {
        if (textArray.length <= 1 || cycleIntervalId) return;

        cycleIntervalId = setInterval(cycleNext, cycleInterval);
    }

    function stopCycle() {
        if (cycleIntervalId) {
            clearInterval(cycleIntervalId);
            cycleIntervalId = null;
        }
    }

    // Lifecycle
    onMount(async () => {
        if (!element) return;

        // Set initial content
        if (textArray.length > 0) {
            element.innerHTML = textArray[currentIndex];
        } else if (text) {
            element.innerHTML = text;
        }

        // Apply initial scramble if needed
        if (scrambleOnMount) {
            if (textArray.length > 0) {
                await setText(textArray[currentIndex]);
            } else if (text) {
                await setText(text);
            }
        }

        // Start cycle if needed
        if (autoStart && textArray.length > 1) {
            startCycle();
        }
    });

    onDestroy(() => {
        stopCycle();
        if (frameRequest) {
            cancelAnimationFrame(frameRequest);
        }
    });
</script>

<div bind:this={element} class="text-scramble">
    {#if !text && textArray.length === 0}
        <slot />
    {/if}
</div>

<style>
    .text-scramble {
        display: inline-block;
    }

    :global(.scramble-char) {
        opacity: 70%;
        display: inline-block;
    }
</style>
