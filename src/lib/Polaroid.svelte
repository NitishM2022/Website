<script>
  import nitishImg from "./assets/n_med.jpeg";
</script>

<div
  class="bg-neutral-100 drop-shadow-lg shadow-black mr-5 mb-8 sm:mb-0 w-54 sm:w-120 dark:shadow-lg dark:shadow-neutral-700 p-5 pb-16 rounded-sm"
>
  <div class="polaroid">
    <!-- Wrap image in div with polaroid class -->
    <img src={nitishImg} alt="Nitish" class="object-cover" />
  </div>
</div>

<style>
  :global(.polaroid) {
    position: relative; /* Add this */
    display: block; /* Add this */
  }

  :global(.polaroid::after) {
    box-shadow: inset 3px 3px 10px 0 #adadad;
    content: "";
    display: block;
    height: 100%;
    position: absolute;
    top: 0;
    width: 100%;
  }
</style>
