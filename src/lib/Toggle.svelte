<script lang="ts">
  import moon from "./assets/moon.svg";
  import sun from "./assets/sun.svg";

  let spinDirection: "left" | "right" = "right";
  let spin = false;

  let darkMode = localStorage.getItem("darkMode")
    ? localStorage.getItem("darkMode") === "true"
    : window.matchMedia?.("(prefers-color-scheme: dark)").matches || false;

  // Apply dark mode to both html and body elements
  if (darkMode) {
    document.documentElement.classList.add("dark");
    document.body.classList.add("dark");
  } else {
    document.documentElement.classList.remove("dark");
    document.body.classList.remove("dark");
  }

  function toggleDarkMode() {
    darkMode = !darkMode;
    if (darkMode) {
      document.documentElement.classList.add("dark");
      document.body.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
      document.body.classList.remove("dark");
    }

    localStorage.setItem("darkMode", darkMode.toString());

    spin = true;
    setTimeout(() => (spin = false), 650);
    spinDirection = spinDirection === "right" ? "left" : "right";
  }
</script>

<!-- Rest of the code stays the same -->

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
      width="18"
      height="18"
      src={moon}
      alt="Enable Light Mode"
      class="icon-moon w-4.5 sm:5"
    />
    <img
      loading="lazy"
      width="18"
      height="18"
      src={sun}
      alt="Enable Dark Mode"
      class="icon-sun w-4.5 sm:5"
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

  :global(body.dark) .icon-moon {
    display: block;
  }

  :global(body:not(.dark)) .icon-sun {
    display: block;
  }
</style>
