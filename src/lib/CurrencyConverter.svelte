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
  <h1>Currency Converter</h1>
  From
  {#await fetchCurrency() then data}
    <select
      bind:value={selectedCurrencyFrom}
      id="selectedCurrencyFrom"
      onchange={handleChange}
    >
      {#each Object.entries(data) as [code, name]}
        <option value={code}
          >{name != ""
            ? `${name} (${code.toLocaleUpperCase()})`
            : code.toUpperCase()}</option
        >
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
        <option value={code}
          >{name != ""
            ? `${name} (${code.toLocaleUpperCase()})`
            : code.toUpperCase()}</option
        >
      {/each}
    </select>
  {:catch error}
    <p>An error occurred!</p>
  {/await}
  <input type="number" id="amountTo" min="0" bind:value={amountTo} readonly />
</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  main {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 300px;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  select,
  input {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    background-color: #f9f9f9;
  }

  select:focus,
  input:focus {
    border-color: #007bff;
    outline: none;
  }

  h1 {
    margin-bottom: 15px;
    font-size: 18px;
    color: #333;
  }

  p {
    color: red;
    font-size: 14px;
  }
</style>
