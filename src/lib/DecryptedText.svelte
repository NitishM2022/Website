<script>
    import { onMount } from "svelte";

    let {
        text,
        speed = 50,
        maxIterations = 10,
        maxLength = 0,
        characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz!@#$%^&*()_+",
        className = "",
        parentClassName = "",
        animateOn = "hover",
        ...props
    } = $props();

    // Always cap the text first
    const cappedText = $derived(
        maxLength > 0 && text.length > maxLength
            ? text.slice(0, maxLength - 3) + "..."
            : text,
    );

    let displayText = $state("");
    let isVisible = $state(false);
    let containerRef = $state(null);
    let intervalId = null;
    let lastText = "";

    function scramble(originalText) {
        const chars = characters.split("");
        return originalText
            .split("")
            .map((char) => {
                if (char === " ") return " ";
                // 8% chance to add a space for natural wrapping
                if (Math.random() < 0.08) return " ";
                return chars[Math.floor(Math.random() * chars.length)];
            })
            .join("")
            .slice(0, originalText.length);
    }

    function runAnimation(targetText) {
        if (intervalId) clearInterval(intervalId);

        let iteration = 0;
        intervalId = setInterval(() => {
            if (iteration >= maxIterations) {
                clearInterval(intervalId);
                intervalId = null;
                displayText = targetText;
                return;
            }
            displayText = scramble(targetText);
            iteration++;
        }, speed);
    }

    onMount(() => {
        displayText = cappedText;
        lastText = cappedText;

        if (animateOn === "view" || animateOn === "both") {
            const observer = new IntersectionObserver(
                ([entry]) => {
                    if (entry.isIntersecting && !isVisible) {
                        isVisible = true;
                        runAnimation(cappedText);
                    }
                },
                { threshold: 0.1 },
            );

            if (containerRef) {
                observer.observe(containerRef);
            }

            return () => {
                observer.disconnect();
                if (intervalId) clearInterval(intervalId);
            };
        }

        return () => {
            if (intervalId) clearInterval(intervalId);
        };
    });

    // Watch for text changes AFTER initial mount
    $effect(() => {
        const currentText = cappedText;
        if (isVisible && lastText !== currentText && lastText !== "") {
            lastText = currentText;
            runAnimation(currentText);
        } else if (!isVisible) {
            displayText = currentText;
            lastText = currentText;
        }
    });

    const isHoverEnabled = $derived(
        animateOn === "hover" || animateOn === "both",
    );
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<span
    bind:this={containerRef}
    class="decrypted-text-wrapper {parentClassName} {className}"
    onmouseenter={() => {
        if (isHoverEnabled) {
            runAnimation(cappedText);
        }
    }}
    {...props}>{displayText}</span
>

<style>
    .decrypted-text-wrapper {
        display: inline;
        white-space: pre-wrap;
        font-family: "Geist Mono", monospace;
    }
</style>
