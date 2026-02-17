<script>
  export let href;
</script>

<a
  role="button"
  {href}
  target="_blank"
  class="flex cursor-pointer flex-row items-center gap-1 hover:opacity-70 hover:duration-100"
>
  <slot />
</a>
