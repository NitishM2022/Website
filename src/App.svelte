<script>
  import Project from "./lib/Project.svelte";
  import Book from "./lib/Book.svelte";
  import Rotext from "./lib/Rotext.svelte";
  import Toggle from "./lib/Toggle.svelte";

  import projects from "./lib/assets/projects.json";
  import nitishImg from "./lib/assets/n_med.jpeg";
  import resume from "./lib/assets/resume.pdf";

  let isDarkMode =
    window.matchMedia &&
    window.matchMedia("(prefers-color-scheme: dark)").matches;

  // Apply the initial dark mode state
  if (isDarkMode) {
    document.documentElement.classList.add("dark");
  } else {
    document.documentElement.classList.remove("dark");
  }

  function toggleDarkMode() {
    isDarkMode = !isDarkMode;
    if (isDarkMode) {
      document.documentElement.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
    }
  }
</script>

<main class="mx-auto px-4 pb-8 w-180 font-rope flex flex-col gap-12 mb-2">
  <!-- header -->
  <div class="flex flex-row items-center mt-4">
    <h1
      class="text-2xl font-rope font-extralight text-stone-950 dark:text-stone-100"
    >
      nitish malluru
    </h1>
    <div class="flex-grow"></div>
    <div
      class="flex flex-row gap-3 text-stone-950 dark:text-stone-100 items-center mr-2"
    >
      <div
        role="button"
        on:click={window.open(resume, "_blank")}
        class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer border-b-1"
      >
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
        <p>RESUME</p>
      </div>

      <div
        role="button"
        on:click={window.open("https://github.com/NitishM2022", "_blank")}
        class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer border-b-1"
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
            d="M15 22v-4a4.8 4.8 0 0 0-1-3.5c3 0 6-2 6-5.5.08-1.25-.27-2.48-1-3.5.28-1.15.28-2.35 0-3.5 0 0-1 0-3 1.5-2.64-.5-5.36-.5-8 0C6 2 5 2 5 2c-.3 1.15-.3 2.35 0 3.5A5.403 5.403 0 0 0 4 9c0 3.5 3 5.5 6 5.5-.39.49-.68 1.05-.85 1.65-.17.6-.22 1.23-.15 1.85v4"
          ></path><path d="M9 18c-4.51 2-5-2-7-2"></path></svg
        >
        <p>GITHUB</p>
      </div>

      <div
        role="button"
        on:click={window.open(
          "https://bitbucket.org/nitishethan/workspace/repositories/",
          "_blank"
        )}
        class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer border-b-1"
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
        <p>BITBUCKET</p>
      </div>

      <div
        role="button"
        on:click={window.open(
          "https://www.linkedin.com/in/nitishmalluru/",
          "_blank"
        )}
        class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer border-b-1 -pb-1"
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
        <p>LINKEDIN</p>
      </div>
    </div>
    <Toggle />
  </div>

  <section class="  flex flex-row">
    <img
      src={nitishImg}
      alt="Nitish"
      class="w-54 h-54 object-cover rounded-lg mr-6"
    />
    <div class="w-full">
      <h1
        class="text-stone-950 dark:text-stone-100 text-5xl font-medium mb-4 font-serif italic"
      >
        Howdy
      </h1>
      <Rotext />
    </div>
  </section>

  <!-- Projects Section -->
  <section class="">
    <h2
      class=" font-medium text-2xl text-stone-950 dark:text-stone-100 pb-2 mb-2"
    >
      Projects
    </h2>
    <div class="grid grid-cols-2 gap-5">
      {#each projects as { name, description, link, date, tech }}
        <Project {name} {description} {link} {date} {tech} />
      {/each}
    </div>
  </section>

  <!-- Reading List -->
  <section>
    <h2
      class="text-2xl font-medium text-stone-950 dark:text-stone-100 pb-2 mb-2"
    >
      Reading
    </h2>
    <div class="font-light flex flex-row gap-4">
      <Book
        name="Conclave"
        author="Richard Harris"
        image="https://upload.wikimedia.org/wikipedia/en/4/42/Conclave_%28novel%29.jpg"
        length="304"
        link="https://www.amazon.com/Conclave-novel-Robert-Harris/dp/0593689585"
      />
      <Book
        name="Speaker For the Dead"
        author="Orson Scott Card"
        image="https://m.media-amazon.com/images/I/51dy+hyokZS._SY445_SX342_.jpg"
        length="382"
        link="https://www.amazon.com/Speaker-Dead-Ender-Quartet-Book-ebook/dp/B003H4I4JU?dib=eyJ2IjoiMSJ9.NHvd5oxu3pHjqiddqIMUGqr-B2YDr0n63GkXq_PV2KvSjiIAGsrLVuWzHkCPdfIj57tI9gzSfO-Qu7-wa4Bg0uoFzot_omQATdxVLod11h4md3hJ0iKsg-MLR56BQ_BAcin568-Y483mT0HtMviXOy3r7n3Kk5MXNDTmpSCT-fZ8xyxxgeljtnzSs2H9WAGyMaIYZmqwhv2quyz2m_yogjrOvTVKpaa1G4fD6SBYY5Y.FBTJlO1ututPH-WSme9YvojbJGyVuO17aYnfTR3APL8&dib_tag=AUTHOR"
      />
      <Book
        name="Deep Learning with Python"
        author="Francois Chollet"
        image="https://m.media-amazon.com/images/I/71sQM8kzchL._SY522_.jpg"
        length="504"
        link="https://www.amazon.com/Learning-Python-Second-Fran%C3%A7ois-Chollet-ebook/dp/B09K81XLN1?crid=2HERLLYS0DJKQ&dib=eyJ2IjoiMSJ9.wUWaDqCxCxhGwuAIYPlTIqPnY4aI6cIKjF4PcarFwOh9UqFniFF3nvgxyjryaGUBTYacPzbVfgFQFkZqqi1LFBPFJ0p4FURKAlOvEWDCo7EKpvg2Zjm2pI-1FltwxY6nGLJJg3pLYCraPySl3QIEc3NoTeHAqSag8I1k1t0jYC0GMETvor10laZYsP7n24TBSDdSeIIDdZcGdIxA0YPO-QDlXeF0ib21duGJRUnhmZc.8nGLn6_GYWd1n_z3b7BOO72mlJxBRNSMfKQzEfljux0&dib_tag=se&keywords=deep+learning+with+python&qid=1735014329&s=digital-text&sprefix=deep+learning+w,digital-text,111&sr=1-1"
      />
    </div>
  </section>

  <!-- Contact Section -->
  <section>
    <h2
      class="font-medium text-2xl text-stone-950 dark:text-stone-100 pb-2 mb-2"
    >
      Contact
    </h2>
    <p>
      <a
        href="mailto:nitishethan@gmail.com"
        class="text-stone-950 dark:text-stone-300 font-light underline"
        >nitishethan@gmail.com</a
      >
    </p>
  </section>
</main>
