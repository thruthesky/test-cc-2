<script>
  const options = [
    { value: 1, label: "USD" },
    { value: 56, label: "PHP" },
    { value: 1.59, label: "AUD" },
  ];
  let amountFrom = $state(0);
  let amountTo = $state();
</script>

<main>
  <select>
    {#each options as option}
      <option value={option.value}>{option.label}</option>
    {/each}
  </select>
  From
  <input type="number" id="amountFrom" min="0" bind:value={amountFrom} />
  To
  <select>
    {#each options as option}
      <option value={option.value}>{option.label}</option>
    {/each}
  </select>

  <button> Convert </button>
  <input type="number" id="amountTo" min="0" bind:value={amountTo} readonly />
</main>
