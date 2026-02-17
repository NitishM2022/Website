<script>
    import { onMount } from "svelte";

    const SCRAMBLE_GLYPHS = "—~±§|[].+$^@*()•x%!?#";

    let {
        text,
        duration = 900,
        maxLength = 0,
        className = "",
        parentClassName = "",
        animateOn = "hover",
        ...props
    } = $props();

    const cappedText = $derived(
        maxLength > 0 && text.length > maxLength
            ? text.slice(0, maxLength - 3) + "..."
            : text,
    );

    let containerRef = $state(null);
    let isVisible = $state(false);
    let renderText = $state("");
    let keyVersion = $state(0);
    let lastText = "";

    function easeInOut(t) {
        return -(Math.cos(Math.PI * t) - 1) / 2;
    }

    function randomGlyph(reverse) {
        if (reverse) return "x";
        return SCRAMBLE_GLYPHS[~~(Math.random() * SCRAMBLE_GLYPHS.length)];
    }

    function replaceChar(str, idx, ch) {
        if (idx < 0 || idx >= str.length) return str;
        return str.substring(0, idx) + ch + str.substring(idx + 1);
    }

    function scramble(node, options = {}) {
        const {
            duration: total = 900,
            reverse = false,
            absolute = false,
            pointerEvents = true,
        } = options;

        const sourceText = node.textContent ?? "";
        const blankText = sourceText.replace(/[^\s]/g, "\u00a0");
        let scrambleBuf = blankText;

        const zoneMax = ~~(sourceText.length * (reverse ? 0.25 : 1.5));
        const persistBias = reverse ? 0.1 : 0.8;

        if (absolute) {
            node.style.position = "absolute";
            node.style.top = "0";
        }

        if (!pointerEvents) {
            node.style.pointerEvents = "none";
        }

        return {
            duration: total,
            tick(progress) {
                let t = easeInOut(progress);
                t = t * t;
                if (reverse) t = 1 - t;

                const activeLen = ~~(sourceText.length * t);
                const zoneWidth = ~~(2 * (0.5 - Math.abs(t - 0.5)) * zoneMax);

                let out = reverse
                    ? blankText.slice(0, Math.max(activeLen - 1 - zoneWidth, 0))
                    : sourceText.slice(0, activeLen);

                if (Math.random() < 0.5 && t < 1 && t !== 0) {
                    for (let i = 0; i < 20; i++) {
                        const drift = i / 20;
                        const idx = activeLen + ~~((1 - Math.random()) * zoneMax * drift);
                        if (idx >= 0 && idx < scrambleBuf.length && scrambleBuf[idx] !== " ") {
                            scrambleBuf =
                                Math.random() > persistBias
                                    ? replaceChar(scrambleBuf, idx, sourceText[idx])
                                    : replaceChar(scrambleBuf, idx, randomGlyph(reverse));
                        }
                    }
                }

                if (reverse) {
                    out += scrambleBuf.slice(
                        Math.max(activeLen - 1 - zoneWidth, 0),
                        Math.max(activeLen - 1, 0),
                    );
                    out += sourceText.slice(Math.max(activeLen - 1, 0));
                } else {
                    out += scrambleBuf.slice(activeLen, activeLen + zoneWidth);
                    out += blankText.slice(activeLen + zoneWidth);
                }

                node.textContent = out;
            },
        };
    }

    function rerun(nextText) {
        renderText = nextText;
        keyVersion += 1;
    }

    onMount(() => {
        const startsOnView = animateOn === "view" || animateOn === "both";
        lastText = cappedText;

        if (!startsOnView) {
            isVisible = true;
            renderText = cappedText;
            return;
        }

        renderText = "\u00a0".repeat(cappedText.length);

        const observer = new IntersectionObserver(
            ([entry]) => {
                if (entry.isIntersecting && !isVisible) {
                    isVisible = true;
                    rerun(cappedText);
                }
            },
            { threshold: 0.1 },
        );

        if (containerRef) {
            observer.observe(containerRef);
        }

        return () => observer.disconnect();
    });

    $effect(() => {
        const currentText = cappedText;

        if (isVisible && currentText !== lastText) {
            lastText = currentText;
            rerun(currentText);
            return;
        }

        if (!isVisible) {
            renderText = "\u00a0".repeat(currentText.length);
            lastText = currentText;
        }
    });

    const isHoverEnabled = $derived(animateOn === "hover" || animateOn === "both");
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<span
    bind:this={containerRef}
    class="decrypted-text-wrapper {parentClassName} {className}"
    onmouseenter={() => {
        if (isHoverEnabled && isVisible) {
            rerun(cappedText);
        }
    }}
    {...props}
>
    {#key keyVersion}
        <span
            class="decrypted-text-content"
            in:scramble={{ duration }}
            out:scramble={{ duration: Math.min(duration * 0.5, 420), reverse: true }}
            >{renderText}</span
        >
    {/key}
</span>

<style>
    .decrypted-text-wrapper {
        display: inline-grid;
        white-space: pre-wrap;
        font-family: "Geist Mono", monospace;
    }

    .decrypted-text-content {
        grid-area: 1 / 1;
    }
</style>
