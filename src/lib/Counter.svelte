<script lang="ts">
  let count: number = 0
  const increment = () => {
    count += 1
  }
</script>

<button class="bg-yellow-500 rounded p-2" on:click={increment}>
  count is {count}
</button>
