<script>
  import { onMount } from "svelte";

  const bioTexts = [
    'studying <span class="rotext"> CS </span> with a minor in <span class="rotext">Embedded Systems</span> at <span class="tamu">Texas A&M</span>.',
    'interested in <span class="rotext"> Graphics </span>, <span class="rotext">ML</span>, and <span class="rotext">Computer Architecture</span>.',
    'who worked at <span class="rotext">Dell</span> where I developed a <span class="rotext">BIOS</span> debugger using <span class="rotext">GenAI</span> and <span class="rotext">RAG</span>.',
    '<span class="silver">Eagle Scout</span>, <span class="rotext"> President\'s Endowed  Scholar</span>, and <span class="gold">USACO Gold</span> divison member.',
  ];

  let currentStep = $state(0);
  let steps;

  function nextStep() {
    const nextStepNum = (currentStep + 1) % steps.length;

    steps[currentStep].classList.remove("set");
    steps[currentStep].classList.add("down");

    steps[nextStepNum].classList.add("set");
    steps[nextStepNum].classList.remove("down");

    currentStep = nextStepNum;
  }

  onMount(() => {
    steps = document.querySelectorAll(".step");

    setInterval(nextStep, 4000); // Adjust interval for desired animation speed
  });
</script>

<div class="font-light text-2xl xs:text-3xl text-stone-950 dark:text-stone-300">
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
  :global(.silver) {
    font-weight: 900;
    background: radial-gradient(
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
    font-weight: 900;
    background: radial-gradient(
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
    font-weight: 900 !important;
    color: #750000;
    background-image: linear-gradient(315deg, #750000 21%, #c14444 81%);
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  :global(.rotext) {
    font-weight: 900 !important;
  }

  .flip {
    position: relative;
    perspective: 500px;
    transition: all 0.3s ease-in-out;
  }

  .flip p {
    transition: all 0.3s ease-in-out;
    opacity: 0;
    transform-origin: 50% 0%;
    transform: rotateX(90deg);
    position: absolute;
  }

  .flip p.set {
    top: 0;
    opacity: 1;
    transform-origin: 50% 0%;
    transform: rotateX(0deg);
  }

  .flip p.down {
    opacity: 0;
    transform-origin: 50% 0%;
    transform: rotateX(-90deg);
  }
</style>
