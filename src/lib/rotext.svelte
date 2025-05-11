<script>
    import { onMount, onDestroy } from "svelte";

    const bioTexts = [
        'studying <span class="rotext"> CS </span> with a minor in <span class="rotext">Embedded Systems</span> at <span class="tamu">Texas A&M</span>.',
        'interested in <span class="rotext"> Graphics</span>, <span class="rotext">Computer Architecture</span>, and <span class="rotext">ML</span>',
        'who worked at <span class="rotext">Dell</span> where I developed a <span class="rotext">BIOS</span> debugger using <span class="rotext">GenAI</span> and <span class="rotext">RAG</span>',
        '<span class="silver">Eagle Scout</span>, <span class="rotext"> President\'s  Scholar</span>, and <span class="gold">USACO Gold</span> divison member',
    ];

    let currentStep = $state(0); // Svelte 5 reactive state
    let animatedTextEl; // bind:this to the paragraph element
    let fx; // Will hold our TextScramble instance

    // Configuration
    const intervalBetweenChanges = 4000; // ms, time before switching to the next bio text
    let cycleIntervalId = null;

    class TextScramble {
        constructor(el) {
            this.el = el;
            this.chars =
                "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
            this.update = this.update.bind(this);
        }

        setText(html) {
            // First extract text-only content of the target element
            // We need to create a temp element to handle HTML properly
            const tempDiv = document.createElement("div");
            tempDiv.innerHTML = this.el.innerHTML || "";
            const oldText = tempDiv.textContent || "";

            // Same for new text
            tempDiv.innerHTML = html;
            const newText = tempDiv.textContent || "";

            // Store the full HTML for final replacement
            this.finalHtml = html;

            const length = Math.max(oldText.length, newText.length);
            const promise = new Promise((resolve) => (this.resolve = resolve));

            this.queue = [];
            for (let i = 0; i < length; i++) {
                const from = oldText[i] || "";
                const to = newText[i] || "";
                const start = Math.floor(Math.random() * 40);
                const end = start + Math.floor(Math.random() * 40);
                this.queue.push({ from, to, start, end });
            }

            cancelAnimationFrame(this.frameRequest);
            this.frame = 0;
            this.update();
            return promise;
        }

        update() {
            let output = "";
            let complete = 0;

            for (let i = 0, n = this.queue.length; i < n; i++) {
                let { from, to, start, end, char } = this.queue[i];

                if (this.frame >= end) {
                    complete++;
                    output += to;
                } else if (this.frame >= start) {
                    if (!char || Math.random() < 0.28) {
                        char = this.randomChar();
                        this.queue[i].char = char;
                    }
                    output += `<span class="scramble-char">${char}</span>`;
                } else {
                    output += from;
                }
            }

            this.el.innerHTML = output;

            if (complete === this.queue.length) {
                // Replace with proper HTML when animation is done
                this.el.innerHTML = this.finalHtml;
                this.resolve();
            } else {
                this.frameRequest = requestAnimationFrame(this.update);
                this.frame++;
            }
        }

        randomChar() {
            return this.chars[Math.floor(Math.random() * this.chars.length)];
        }
    }

    async function cycleBioText() {
        currentStep = (currentStep + 1) % bioTexts.length;
        const nextHtml = bioTexts[currentStep];

        if (fx && animatedTextEl) {
            await fx.setText(nextHtml);
        }
    }

    onMount(() => {
        if (animatedTextEl) {
            // Create the TextScramble instance
            fx = new TextScramble(animatedTextEl);

            // Set initial content
            animatedTextEl.innerHTML = bioTexts[currentStep];

            // Start first animation with a slight delay for better UX
            setTimeout(() => {
                fx.setText(bioTexts[currentStep]).then(() => {
                    // Start cycling after the first animation is complete
                    if (bioTexts.length > 1) {
                        cycleIntervalId = setInterval(
                            cycleBioText,
                            intervalBetweenChanges,
                        );
                    }
                });
            }, 300);
        }
    });

    onDestroy(() => {
        if (cycleIntervalId) {
            clearInterval(cycleIntervalId);
        }
        if (fx && fx.frameRequest) {
            cancelAnimationFrame(fx.frameRequest);
        }
    });
</script>

<div
  class="font-light text-2xl xs:leading-10 xs:text-[30px] text-stone-950 dark:text-stone-300"
>
  {#if currentStep == 3}
    <p>
      I'm Nitish, an aspiring <a
        href="https://paulgraham.com/hp.html"
        target="_blank"
        class="underline underline-offset-4 decoration-2">hacker</a
      >,
    </p>
  {:else}
    <p>
      I'm Nitish, an aspiring <a
        href="https://paulgraham.com/hp.html"
        target="_blank"
        class="underline underline-offset-4 decoration-2">hacker</a
      >
    </p>
  {/if}
  <div class="flip">
    {#each bioTexts as text, index}
      {#if index === 0}
        <p class="step step0 set">{@html text}</p>
      {:else}
        <p class={"step step" + index}>{@html text}</p>
      {/if}
    {/each}
  </div>
</div>

<style>
    /* Keep your existing :global styles for .rotext, .tamu, .silver, .gold etc. */
    :global(.hero-heading) {
        background: radial-gradient(
                40% 64% at 13% 100%,
                hsla(79, 59%, 81%, 0.8),
                #fff
            )
            text;
        -webkit-box-decoration-break: clone;
        -webkit-text-fill-color: transparent;
        color: unset;
        padding-bottom: 0.13em;
        line-height: 0.9;
        letter-spacing: -0.01em;
        font-weight: 450;
    }

    :global(.silver) {
        /* font-weight: 900; */
        background:
            radial-gradient(
                ellipse farthest-corner at right bottom,
                #b8b8b8 10%,
                #7a7a7a 40%,
                transparent 80%
            ),
            radial-gradient(
                ellipse farthest-corner at left top,
                #d4d4d4 5%,
                #909090 62.5%,
                #757575 100%
            );
        -webkit-background-clip: text;
        background-clip: text;
        color: transparent;
    }

    :global(.gold) {
        /* font-weight: 900; */
        background:
            radial-gradient(
                ellipse farthest-corner at right bottom,
                #fedb37 0%,
                #fdb931 8%,
                #9f7928 30%,
                #8a6e2f 40%,
                transparent 80%
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

    :global(.tamu) {
        /* font-weight: 900 !important; */
        color: #750000;
        background-image: linear-gradient(315deg, #750000 21%, #c14444 81%);
        background-clip: text;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    :global(.rotext) {
        opacity: 50%;
    }

    .bio-text-container {
        position: relative;
        min-height: 5rem;
        display: flex;
        align-items: center;
    }

    .bio-content {
        line-height: inherit;
        width: 100%;
    }

    :global(.scramble-char) {
        /* font-weight: bold; */
        opacity: 50%;
        display: inline-block;
    }
</style>
