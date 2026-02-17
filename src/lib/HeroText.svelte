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
  class="xs:text-3xl h-56 overflow-hidden text-2xl leading-[1.18] tracking-tight text-stone-100 uppercase transition-all duration-300 ease-in-out md:h-60 md:text-[2.5rem] md:leading-[1.2]"
  style="font-family: 'Geist Mono', monospace;"
>
  I'm Nitish, a <a
    href="https://paulgraham.com/hp.html"
    target="_blank"
    class="hero-link underline decoration-2 underline-offset-4">builder</a
  >,
  <DecryptedText
    text={bioTexts[currentIndex]}
    animateOn="view"
    duration={900}
    {maxLength}
  />
</div>
