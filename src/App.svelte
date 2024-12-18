<script>
  export let githubLink = "https://github.com/AidenBeresford/FeGBA";

  function redirect(event, link) {
    event.preventDefault(); // Prevent default link behavior
    window.open(link, "_blank", "noopener,noreferrer");
  }
</script>

<main class="mx-auto mt-6 px-4 py-8 w-180 font-sans">
  <!-- Header Section -->
  <section class="flex flex-row gap-6 mb-10">
    <img
      src="src/assets/n.jpeg"
      alt="Nitish"
      class="w-48 h-48 object-cover rounded-lg"
    />
    <div class="flex flex-col gap-2 justify-center">
      <h1 class="text-4xl tracking-wide text-stone-950 font-mono">
        Nitish Malluru
      </h1>
      <div class="flex flex-row gap-3 text-stone-500 items-center">
        <div
          role="button"
          on:click={window.open("/public/resume.pdf", "_blank")}
          class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer"
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
          class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer"
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
          class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer"
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
          class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer"
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
    </div>
  </section>

  <!-- About Me -->
  <section class="mb-10">
    <h2 class="text-xl text-stone-950 pb-2 mb-2">About Me</h2>
    <div class="space-y-8 font-light">
      <div class="text-stone-950 space-y-2">
        <p>
          🌱 <strong>Interests:</strong> Honors Computer Science @ Texas A&M University
          with minor in Embedded Systems, interested in Graphics, Machine Learning,
          and Computer Architecture.
        </p>
        <p>
          🏋️‍♀️ <strong>Previous Work:</strong> Developed a BIOS debugging chatbot using
          GenAI and RAG at Dell.
        </p>
        <p>
          🎖️ <strong>Achievements:</strong>
          Eagle Scout, President’s Endowed Scholar, and USA Computing Olympiad Gold
          Division.
        </p>
        <p>
          🚀 <strong>Goals:</strong> Aspiring founder, youtuber, and guitarist.
        </p>
      </div>
    </div>
  </section>

  <!-- Projects Section -->
  <section class="mb-10">
    <h2 class="text-xl text-stone-950 pb-2 mb-2">Projects</h2>
    <div class="space-y-2 font-light">
      <div
        role="button"
        on:click={window.open(
          "https://github.com/AidenBeresford/FeGBA",
          "_blank"
        )}
        class="hover:opacity-70 hover:duration-100 cursor-pointer"
      >
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">FeGBA</h3>
          <p class="text-xs text-stone-500 font-mono italic">PRESENT</p>
        </div>
        <p class="text-sm text-stone-500 text-left">
          A Game Boy Advance emulator written in Rust, designed to replicate the
          hardware and gameplay of the original console. Implements CPU, memory,
          graphics, and audio to run GBA games accurately.
        </p>
      </div>
      <div
        role="button"
        on:click={window.open(
          "https://github.com/NitishM2022/MiniOS",
          "_blank"
        )}
        class="hover:opacity-70 hover:duration-100 cursor-pointer"
      >
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">MiniOS</h3>
          <p class="text-xs text-stone-500 font-mono italic">12/2024</p>
        </div>
        <p class="text-sm text-stone-500">
          A rudimentary operating system built on QEMU with virtual memory,
          multithreading, hardware drivers, and a file system. Required
          knowledge of memory paging, thread scheduling, device I/O, storage,
          and x86 Assembly.
        </p>
      </div>
      <div
        role="button"
        on:click={window.open(
          "https://github.com/NitishM2022/Networking/tree/main/tcp_over_udp",
          "_blank"
        )}
        class="hover:opacity-70 hover:duration-100 cursor-pointer"
      >
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">TCP over UDP</h3>
          <p class="text-xs text-stone-500 font-mono italic">11/2024</p>
        </div>
        <p class="text-sm text-stone-500">
          Application-layer implementation of a connection-oriented protocol
          over a connectionless one. Emulates flow control, congestion control,
          RTT estimation, fast retransmits, and more, achieving over 1 Gbps
          throughput with threading.
        </p>
      </div>
      <div
        role="button"
        on:click={window.open(
          "https://github.com/NitishM2022/Graphics/tree/main/ray_tracer",
          "_blank"
        )}
        class="hover:opacity-70 hover:duration-100 cursor-pointer"
      >
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">A Simple Ray Tracer</h3>
          <p class="text-xs text-stone-500 font-mono italic">04/2024</p>
        </div>
        <p class="text-sm text-stone-500">
          Simulates light behavior to accurately reproduce shadows, reflections,
          and material interactions. Built with C++, the program calculates
          light paths for each pixel to create lifelike visuals.
        </p>
      </div>
      <!---
      <div class="hover:opacity-70 hover:duration-100">
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">ArborWorx</h3>
          <p class="text-xs text-stone-500 font-mono italic">01/2024</p>
        </div>
        <p class="text-sm text-stone-500">
          Software solutions built for small businesses to manage customer
          credit info. Focused on data collection and processing using Python
          and Java. These tools led to efficienty gains of 2000%.
        </p>
      </div>-->
      <div
        role="button"
        on:click={window.open(
          "https://github.com/NitishM2022/NandtoTetris/tree/main/cpu_16bit",
          "_blank"
        )}
        class="hover:opacity-70 hover:duration-100 cursor-pointer"
      >
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">16 Bit CPU</h3>
          <p class="text-xs text-stone-500 font-mono italic">10/2023</p>
        </div>
        <p class="text-sm text-stone-500">
          Designed a 16-bit CPU in with a custom instruction set using HDL and
          Nand2Tetris. Built ALU, program counter, dissasembler, registers, and
          memory with basic AND, OR, NOT gates.
        </p>
      </div>
      <div
        role="button"
        on:click={window.open(
          "https://github.com/NitishM2022/PocketVAX",
          "_blank"
        )}
        class="hover:opacity-70 hover:duration-100 cursor-pointer"
      >
        <div class="flex items-center justify-between space-x-4">
          <h3 class="text-stone-950">PocketVAX</h3>
          <p class="text-xs text-stone-500 font-mono italic">07/2022</p>
        </div>
        <p class="text-sm text-stone-500">
          An immunization record-keeping app built with React Native. Offers
          user-friendly design, secure data storage, vaccine reminders, detailed
          vaccine info, and record-sharing with healthcare providers.
        </p>
      </div>
    </div>
  </section>

  <!-- Reading List -->
  <section class="mb-10">
    <h2 class="text-xl text-stone-950 pb-2 mb-2">Reading</h2>
    <div class="font-light">
      <h3 class=" text-stone-950">Conclave</h3>
      <h3 class=" text-stone-950">Speaker For the Dead</h3>
      <h3 class=" text-stone-700">The Hundred Page Machine Learning Book</h3>
    </div>
  </section>

  <!-- Contact Section -->
  <section>
    <h2 class="text-xl text-stone-950 pb-2 mb-2">Contact</h2>
    <p>
      <a
        href="mailto:nitishethan@gmail.com"
        class="text-stone-950 font-light hover:underline"
        >nitishethan@gmail.com</a
      >
    </p>
  </section>
</main>
