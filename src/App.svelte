<script>
  import { fade, fly } from "svelte/transition";
  import Project from "./lib/Project.svelte";
  import HeroText from "./lib/HeroText.svelte";
  import Toggle from "./lib/Toggle.svelte";
  import Link from "./lib/Link.svelte";
  import Shader from "./lib/Shader.svelte";
  import BooksSection from "./lib/BooksSection.svelte";
  import HeroPixelHover from "./lib/HeroPixelHover.svelte";
  import AsciiText from "./lib/AsciiText.svelte";
  import projects from "./lib/assets/projects.json";
</script>

<div class="flex items-stretch">
  <div aria-hidden="true" class="hidden lg:flex lg:flex-1 lg:flex-col">
    <div class="grid-h h-10"></div>
    <div class="grid-dash-b-long-right flex-1"></div>
  </div>
  <div class="md:grid-lr-b relative w-full lg:w-270 lg:flex-none">
    <div
      class="relative isolate z-10 mb-[1px] overflow-hidden bg-[#0041ba] sm:mx-[1px]"
    >
      <HeroPixelHover alwaysOn={true} />

      <div
        class="relative z-10 flex h-10 items-center justify-center px-3 md:px-5"
      >
        <h1 class="font-rope text-2xl font-light text-stone-100">
          nitish malluru
        </h1>
        <div class="flex-grow"></div>
        <div class="mr-2 flex items-center gap-3 text-stone-100">
          <Link href="./resume.pdf">
            <i class="ri-profile-line align-middle text-xl"></i>
          </Link>
          <Link href="mailto:nitishethan@gmail.com">
            <i class="ri-mail-line align-middle text-xl"></i>
          </Link>
          <Link href={"https://github.com/NitishM2022"}>
            <i class="ri-github-line align-middle text-xl" aria-hidden="true"
            ></i>
          </Link>
          <Link href={"https://www.linkedin.com/in/nitishmalluru/"}>
            <i class="ri-linkedin-box-line align-middle text-xl"></i>
          </Link>
        </div>
        <Toggle />
      </div>

      <section
        class="relative z-10 mt-1 flex h-auto w-full flex-col overflow-visible md:flex-row md:overflow-hidden"
      >
        <div
          class="mb-1.5 box-border h-72 w-full overflow-hidden rounded-tr-3xl rounded-bl-3xl pr-2 pb-2 pl-3 md:mr-5 md:mb-5 md:aspect-square md:h-auto md:w-[45%] md:flex-none md:self-start md:pr-0 md:pb-0 md:pl-5"
        >
          <Shader />
        </div>

        <div
          class="mb-0 flex w-full flex-col justify-center pr-5 md:mb-5 md:h-auto md:flex-1"
        >
          <div
            class="h-30 min-h-[8rem] w-full items-center pb-1 pl-3 md:h-30 md:min-h-[10rem] md:pl-0 lg:h-50"
          >
            <HeroText />
          </div>
        </div>
      </section>
    </div>
  </div>
  <div aria-hidden="true" class="hidden lg:flex lg:flex-1 lg:flex-col">
    <div class="grid-h h-10"></div>
    <div class="grid-dash-b-long flex-1"></div>
  </div>
</div>

<div class="main-content">
  <div class="flex w-full justify-center overflow-hidden">
    <div class="mx-auto flex w-max flex-row justify-center">
      <div
        aria-hidden="true"
        class="md:grid-dot-dash-l-long w-[120px] flex-none opacity-100"
      ></div>
      <!-- Projects Section -->
      <main
        class="font-rope md:grid-dash-lr-long flex w-screen max-w-[840px] flex-none flex-col"
      >
        <section class="" in:fly={{ y: 50, duration: 800, delay: 100 }}>
          <h2
            class="mt-5 mb-5 px-4 text-2xl leading-10 font-medium text-stone-950 dark:text-stone-100"
          >
            Projects
          </h2>
          <div class="grid gap-10 md:grid-cols-1">
            {#each projects as { name, description, link, date, tech, images }}
              <Project {name} {description} {link} {date} {tech} {images} />
            {/each}
          </div>
        </section>

        <!-- Reading List -->
        <div in:fly={{ y: 50, duration: 800, delay: 200 }} class="">
          <BooksSection />
        </div>
      </main>
      <div
        aria-hidden="true"
        class="grid-dot-dash-r-long w-[120px] flex-none opacity-100"
      ></div>
    </div>
  </div>
</div>

<!-- Contact Section -->

<footer class="grid-f-lr-lines h-10 bg-[#0041ba]">
  <div
    class="md:grid-f-lr-lines mx-auto flex h-full items-center bg-[#0041ba] px-5 lg:w-270"
    style="--grid-line-color: rgba(255, 255, 255, 0.15);"
  >
    <p class="text-base font-light text-stone-100">
      <span class="xs:inline hidden">Reach me at&nbsp;</span><a
        href="mailto:nitishethan@gmail.com"
        class="text-stone-100 underline visited:text-stone-100 hover:text-white"
        >nitishethan@gmail.com</a
      >
    </p>
    <div class="flex-grow"></div>
    <p class="text-base font-light text-stone-100">Peace 🚬</p>
  </div>
</footer>
