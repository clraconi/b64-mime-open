<script context="module">
  export const prerender = true;
</script>

<script>
  import B64Opener from '$lib/B64Opener.svelte';
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <B64Opener />
</section>

<style>
</style>
