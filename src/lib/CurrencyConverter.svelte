<script>
  const fetchCurrency = (async () => {
    const response = await fetch(
      "https://cdn.jsdelivr.net/npm/@fawazahmed0/currency-api@latest/v1/currencies.json"
    );
    const data = await response.json();
    return data;
  })();

  let amountFrom = $state(0);
  let amountTo = $state();
  let selectedCurrencyFrom = $state();
  let selectedCurrencyTo = $state();
</script>

<main>
  From
  {#await fetchCurrency then data}
    <select bind:value={selectedCurrencyFrom} id="selectedCurrencyFrom">
      {#each Object.entries(data) as [code, name]}
        <option value={code}>{name} ({code})</option>
      {/each}
    </select>
  {:catch error}
    <p>An error occurred!</p>
  {/await}
  <input type="number" id="amountFrom" min="0" bind:value={amountFrom} />
  To

  {#await fetchCurrency then data}
    <select bind:value={selectedCurrencyTo} id="selectedCurrencyTo">
      {#each Object.entries(data) as [code, name]}
        <option value={code}>{name} ({code})</option>
      {/each}
    </select>
  {:catch error}
    <p>An error occurred!</p>
  {/await}
  <button> Convert </button>
  <input type="number" id="amountTo" min="0" bind:value={amountTo} readonly />
</main>
