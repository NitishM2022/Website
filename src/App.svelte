<script>
  import Project from "./lib/Project.svelte";
  import Book from "./lib/Book.svelte";
  import Rotext from "./lib/Rotext.svelte";
  import Toggle from "./lib/Toggle.svelte";
  import Link from "./lib/Link.svelte";
  import Shader from "./lib/Shader.svelte";

  import projects from "./lib/assets/projects.json";
  import books from "./lib/assets/books.json";
</script>

<main
  class="mx-auto mt-6 mb-10 px-8 sm:px-0 w-full sm:w-190 font-rope flex flex-col gap-5"
>
  <section class="flex flex-col gap-6 rounded-3xl">
    <div class="flex flex-row items-center">
      <h1
        class="text-2xl font-rope font-extralight text-stone-950 dark:text-stone-100"
      >
        nitish malluru
      </h1>
      <div class="flex-grow"></div>
      <div
        class="flex flex-row gap-3 text-stone-950 dark:text-stone-100 items-center mr-2"
      >
        <Link href="./resume.pdf" name="RESUME">
          <svg
            width="16"
            height="16"
            viewBox="0 0 15 15"
            stroke="currentColor"
            stroke-width="0.5"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            ><path
              d="M7.5 0.875C5.49797 0.875 3.875 2.49797 3.875 4.5C3.875 6.15288 4.98124 7.54738 6.49373 7.98351C5.2997 8.12901 4.27557 8.55134 3.50407 9.31167C2.52216 10.2794 2.02502 11.72 2.02502 13.5999C2.02502 13.8623 2.23769 14.0749 2.50002 14.0749C2.76236 14.0749 2.97502 13.8623 2.97502 13.5999C2.97502 11.8799 3.42786 10.7206 4.17091 9.9883C4.91536 9.25463 6.02674 8.87499 7.49995 8.87499C8.97317 8.87499 10.0846 9.25463 10.8291 9.98831C11.5721 10.7206 12.025 11.8799 12.025 13.5999C12.025 13.8623 12.2376 14.0749 12.5 14.0749C12.7623 14.075 12.975 13.8623 12.975 13.6C12.975 11.72 12.4778 10.2794 11.4959 9.31166C10.7244 8.55135 9.70025 8.12903 8.50625 7.98352C10.0187 7.5474 11.125 6.15289 11.125 4.5C11.125 2.49797 9.50203 0.875 7.5 0.875ZM4.825 4.5C4.825 3.02264 6.02264 1.825 7.5 1.825C8.97736 1.825 10.175 3.02264 10.175 4.5C10.175 5.97736 8.97736 7.175 7.5 7.175C6.02264 7.175 4.825 5.97736 4.825 4.5Z"
              fill="currentColor"
              fill-rule="evenodd"
              clip-rule="evenodd"
            ></path></svg
          >
        </Link>
        <Link href="mailto:nitishethan@gmail.com" name="email">
          <svg
            width="19"
            height="19"
            class="text-2xl text-black dark:text-neutral-100"
            data-icon="mdi:email-outline"
          >
            <symbol id="ai:mdi:email-outline" viewBox="0 0 24 24"
              ><path
                fill="currentColor"
                d="M22 6c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2zm-2 0l-8 5l-8-5zm0 12H4V8l8 5l8-5z"
              ></path></symbol
            ><use href="#ai:mdi:email-outline"></use>
          </svg>
        </Link>
        <Link href={"https://github.com/NitishM2022"} name="GITHUB">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2.5"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="w-4 h-4"
            aria-hidden="true"
            ><path
              d="M15 22v-4a4.8 4.8 0 0 0-1-3.5c3 0 6-2 6-5.5.08-1.25-.27-2.48-1-3.5.28-1.15.28-2.35 0-3.5 0 0-1 0-3 1.5-2.64-.5-5.36-.5-8 0C6 2 5 2 5 2c-.3 1.15-.3 2.35 0 3.5A5.403 5.403 0 0 0 4 9c0 3.5 3 5.5 6 5.5-.39.49-.68 1.05-.85 1.65-.17.6-.22 1.23-.15 1.85v4"
            ></path><path d="M9 18c-4.51 2-5-2-7-2"></path></svg
          >
        </Link>

        <Link
          href={"https://bitbucket.org/nitishethan/workspace/repositories/"}
          name="BITBUCKET"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="14"
            height="14"
            stroke-width="0.1"
            viewBox="8.4 14.39 2481.29 2231.21"
            ><path
              d="M81.2602 1.16415e-10C37.3602 -0.569994 1.31016 34.5604 0.750158 78.46C0.690158 83.07 1.03016 87.68 1.77016 92.23L339.25 2140.95C347.93 2192.7 392.51 2230.75 444.99 2231.19H2064.02C2103.4 2231.69 2137.21 2203.29 2143.51 2164.41L2481 92.63C2488.03 49.2904 2458.59 8.45991 2415.25 1.43C2410.7 0.690006 2406.1 0.350002 2401.49 0.410001L81.2602 1.16415e-10ZM1880 1961L633 1960.99L301.5 270.99L2181 271L1880 1961Z"
              fill="currentColor"
            /></svg
          >
        </Link>

        <Link
          href={"https://www.linkedin.com/in/nitishmalluru/"}
          name="LINKEDIN"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="w-4 h-4"
            aria-hidden="true"
            ><path
              d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"
            ></path><rect x="2" y="9" width="4" height="12"></rect><circle
              cx="4"
              cy="4"
              r="2"
            ></circle></svg
          >
        </Link>
      </div>
      <Toggle />
    </div>

    <section
      class="fade-in-bottom-first flex flex-col-reverse sm:flex-row sm:pl-2"
    >
      <div class="sm:w-100 mb-26 xs:mb-30 mt-5 sm:mt-2.5 sm:mb-0">
        <h1
          class="text-stone-950 dark:text-stone-100 text-7xl sm:text-8xl font-medium mb-3 font-serif italic"
        >
          Howdy
        </h1>
        <Rotext />
      </div>
      <div class="flex-grow"></div>

      <Shader />
    </section>
  </section>
  <!-- Projects Section -->
  <section class="fade-in-bottom-second">
    <h2
      class="font-medium text-2xl leading-10 text-stone-950 dark:text-stone-100 mb-5"
    >
      Projects
    </h2>
    <div class="grid sm:grid-cols-1 gap-10">
      {#each projects as { name, description, link, date, tech, images }}
        <Project {name} {description} {link} {date} {tech} {images} />
      {/each}
    </div>
  </section>

  <!-- Reading List -->
  <section class="fade-in-bottom-third">
    <h2
      class=" font-medium text-2xl leading-10 text-stone-950 dark:text-stone-100 mt-10 mb-5"
    >
      Reading
    </h2>
    <div
      class="font-light flex flex-row gap-5 overflow-scroll scrollbar-hidden"
    >
      {#each books as { name, author, image, length, link }, index}
        <div class="min-w-60">
          <Book {name} {author} {image} {length} {link} />
        </div>
      {/each}
    </div>
  </section>

  <!-- Contact Section -->
  <section class="fade-in-bottom-fourth">
    <h2
      class="font-medium text-2xl leading-10 text-stone-950 dark:text-stone-100 mt-10 mb-5"
    >
      Contact
    </h2>
    <p class="text-stone-950 dark:text-stone-300 font-light leading-10 text-lg">
      Reach me at <a href="mailto:nitishethan@gmail.com" class="underline"
        >nitishethan@gmail.com</a
      >
    </p>
    <p class="text-stone-950 dark:text-stone-300 font-light leading-10 text-lg">
      Made with ❤️‍🔥 by Nitish Malluru
    </p>
  </section>
</main>

<style>
  @keyframes fadeInBottom {
    from {
      opacity: 0;
      transform: translateY(60px); /* Reduced from 80px for subtler movement */
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .fade-in-bottom-first {
    animation: fadeInBottom 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-fill-mode: both;
  }

  .fade-in-bottom-second {
    animation: fadeInBottom 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-fill-mode: both;
    animation-delay: 0.1s; /* Shorter delay for tighter sequence */
  }

  .fade-in-bottom-third {
    animation: fadeInBottom 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-fill-mode: both;
    animation-delay: 0.2s;
  }

  .fade-in-bottom-fourth {
    animation: fadeInBottom 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-fill-mode: both;
    animation-delay: 0.3s;
  }
</style>
