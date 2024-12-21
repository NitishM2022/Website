<script lang="ts">
  import moon from "./assets/moon.svg";
  import sun from "./assets/sun.svg";

  let spinDirection: "left" | "right" = "right";
  let spin = false;

  let darkMode =
    window.matchMedia &&
    window.matchMedia("(prefers-color-scheme: dark)").matches;

  // Apply the initial dark mode state
  if (darkMode) {
    document.documentElement.classList.add("dark");
  } else {
    document.documentElement.classList.remove("dark");
  }

  function triggerSpin(duration: number) {
    spin = true;
    setTimeout(() => (spin = false), duration);

    // Alternate spin direction on each click
    spinDirection = spinDirection === "right" ? "left" : "right";
  }

  // Apply the initial dark mode state
  function toggleDarkMode() {
    darkMode = !darkMode;
    if (darkMode) {
      document.documentElement.classList.add("dark");
      document.body.classList.remove("light");
    } else {
      document.documentElement.classList.remove("dark");
      document.body.classList.add("light");
    }

    triggerSpin(650);
  }
</script>

<button
  on:click={toggleDarkMode}
  class="bg-transparent border-0 cursor-pointer p-0 outline-none"
>
  <div
    class="icon"
    class:spin
    class:spin-left={spinDirection === "left"}
    class:spin-right={spinDirection === "right"}
  >
    <img
      loading="lazy"
      width="22"
      height="22"
      src={moon}
      alt="Enable Light Mode"
      class="icon-moon"
    />
    <img
      loading="lazy"
      width="22"
      height="22"
      src={sun}
      alt="Enable Dark Mode"
      class="icon-sun"
    />
  </div>
</button>

<style>
  button {
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
    outline: 0;
  }

  .spin {
    animation: spin 575ms cubic-bezier(0.075, 0.82, 0.17, 1.135);
  }

  .spin-right {
    animation: spin-right 575ms cubic-bezier(0.075, 0.82, 0.17, 1.135);
  }

  .spin-left {
    animation: spin-left 575ms cubic-bezier(0.075, 0.82, 0.17, 1.135);
  }

  @keyframes spin-right {
    0% {
      transform: scale(0) rotate(0deg);
    }
    100% {
      transform: scale(1) rotate(720deg);
    }
  }

  @keyframes spin-left {
    0% {
      transform: scale(0) rotate(0deg);
    }
    100% {
      transform: scale(1) rotate(-720deg);
    }
  }

  img {
    display: none;
  }

  :global(.dark) .icon-moon {
    display: block;
  }

  :global(.light) .icon-sun {
    display: block;
  }
</style>
