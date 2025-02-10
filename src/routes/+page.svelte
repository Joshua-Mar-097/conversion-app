<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let amount = 0;
    let fromCurrency = "USD";
    let toCurrency = "EUR";
    let convertedAmount = null;
    let currencyOptions = [];
    let exchangeRates = {};
  
    const API_KEY = "cur_live_rehxIF0tUUfK00LAhXXlCFuw3i7is1NE4Pza3wkX";
    const API_URL = `https://api.currencyapi.com/v3/latest?apikey=${API_KEY}`;
  
    onMount(async () => {
      try {
        const response = await axios.get(API_URL);
        exchangeRates = response.data.data;
        currencyOptions = Object.keys(exchangeRates);
      } catch (error) {
        console.error("Error fetching exchange rates:", error);
      }
    });
  
    function convertCurrency() {
      if (amount && fromCurrency && toCurrency && exchangeRates[fromCurrency] && exchangeRates[toCurrency]) {
        const result = (amount / exchangeRates[fromCurrency].value) * exchangeRates[toCurrency].value;
        convertedAmount = result.toFixed(2);
      }
    }
  </script>
  
  <style>
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow-x: hidden;
    }
  
    .container {
      min-height: 100vh;
      width: 100vw;
      background: linear-gradient(to right, #e0f7fa, #b2ebf2);
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 1rem;
      box-sizing: border-box;
    }
  
    .card {
      background: white;
      padding: 1.5rem;
      max-width: 500px;
      width: 100%;
      border-radius: 16px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
  
    .result {
      margin-top: 1rem;
      padding: 1rem;
      background: #d4f5e9;
      text-align: center;
      border-radius: 8px;
    }
  
    input, select, button {
      width: 100%;
      padding: 0.75rem;
      margin-bottom: 1rem;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
  
    button {
      background-color: #007bff;
      color: white;
      cursor: pointer;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  </style>
  
  <div class="container">
    <div class="card">
      <h1 class="text-2xl font-bold mb-4 text-center">Money Converter App</h1>
  
      <input type="number" bind:value={amount} placeholder="Enter amount" />
  
      <select bind:value={fromCurrency}>
        {#each currencyOptions as currency}
          <option value={currency}>{currency}</option>
        {/each}
      </select>
  
      <select bind:value={toCurrency}>
        {#each currencyOptions as currency}
          <option value={currency}>{currency}</option>
        {/each}
      </select>
  
      <button on:click={convertCurrency}>Convert</button>
  
      {#if convertedAmount}
        <div class="result">
          <p>Converted Amount: {convertedAmount} {toCurrency}</p>
        </div>
      {/if}
    </div>
  </div>
  