<script>
  import { onMount, onDestroy } from "svelte";
  import DecryptedText from "./DecryptedText.svelte";

  const bioTexts = [
    "Honors CS & Embedded Student at Texas A&M",
    "Founder and SDE Intern {Amazon Luna, Dell}",
    "Eagle Scout, USACO Gold Recipient, and PE Scholar",
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
  class="overflow-hidden text-[1.5rem] leading-[1.18] tracking-tight text-stone-100 uppercase transition-all duration-300 ease-in-out sm:text-[1.9rem] lg:text-[2.5rem]"
  style="font-family: 'Krypton', monospace;"
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
