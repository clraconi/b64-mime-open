<script>
  export let histories = [];
  export let onDelete;

  function handleDelete(item) {
    if (onDelete) {
      onDelete(item);
    }
  }
</script>

<style>
  li {
    display: flex;
    justify-content: space-between;
  }

  li:nth-child(2n) {
    background: #ddd;
  }
</style>

<ul>
{#each histories as item}
  <li>
    <span><a href={item} target="_blank">{item}</a></span>
    <span on:click={() => handleDelete(item)}>&times;</span>
  </li>
{/each}
</ul>

