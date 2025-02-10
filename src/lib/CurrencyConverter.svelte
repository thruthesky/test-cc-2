<script>
  const fetchCurrency = async () => {
    const response = await fetch(
      "https://cdn.jsdelivr.net/npm/@fawazahmed0/currency-api@latest/v1/currencies.json"
    );
    const data = await response.json();
    return data;
  };
  let rate = $state();
  let amountFrom = $state();
  let amountTo = $state();
  let selectedCurrencyFrom = $state("php");
  let selectedCurrencyTo = $state("usd");
  let convertionRate = $state();

  const fetchCurrencyRate = async (selectedCurrencyFrom) => {
    const response = await fetch(
      `https://cdn.jsdelivr.net/npm/@fawazahmed0/currency-api@latest/v1/currencies/${selectedCurrencyFrom}.json`
    );
    const data = await response.json();
    return data[selectedCurrencyFrom];
  };
  $effect(() => {
    if (selectedCurrencyFrom !== undefined) {
      convertionRate = fetchCurrencyRate(selectedCurrencyFrom);
    }
  });

  const handleChange = async () => {
    const reponse = await convertionRate;
    rate = reponse[selectedCurrencyTo];
    amountTo = amountFrom * rate;
  };
</script>

<main>
  From
  {#await fetchCurrency() then data}
    <select
      bind:value={selectedCurrencyFrom}
      id="selectedCurrencyFrom"
      onchange={handleChange}
    >
      {#each Object.entries(data) as [code, name]}
        <option value={code}>{name} ({code})</option>
      {/each}
    </select>
  {:catch error}
    <p>An error occurred!</p>
  {/await}
  <input
    type="number"
    id="amountFrom"
    min="0"
    bind:value={amountFrom}
    oninput={handleChange}
  />
  To
  {#await fetchCurrency() then data}
    <select
      bind:value={selectedCurrencyTo}
      id="selectedCurrencyTo"
      onchange={handleChange}
    >
      {#each Object.entries(data) as [code, name]}
        <option value={code}>{name} ({code})</option>
      {/each}
    </select>
  {:catch error}
    <p>An error occurred!</p>
  {/await}
  <input type="number" id="amountTo" min="0" bind:value={amountTo} readonly />
</main>
