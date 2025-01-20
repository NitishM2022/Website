<script>
  export let href;
  export let name;
</script>

<a
  role="button"
  {href}
  target="_blank"
  class="flex flex-row gap-1 items-center hover:opacity-70 hover:duration-100 cursor-pointer"
>
  <slot />
  <!-- <span class="hidden sm:inline">{name}</span> -->
</a>
