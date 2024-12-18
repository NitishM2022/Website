<script>
  export let name;
  export let description;
  export let link;
  export let date;
</script>

<div
  class="hover:opacity-70 hover:duration-100 cursor-pointer"
  class:hover={!link ? "hover:opacity-100 cursor-default" : ""}
  on:click={() => link && window.open(link, "_blank")}
>
  <div class="flex items-center justify-between space-x-4">
    <h3 class="text-stone-950">{name}</h3>
    <p class="text-xs text-stone-500 font-mono italic">
      {date}
    </p>
  </div>
  <p class="text-sm text-stone-500">{description}</p>
</div>
