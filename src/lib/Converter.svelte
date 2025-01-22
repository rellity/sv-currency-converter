<script lang="ts">
    import { CurrencyConverter } from "../functions/cc";
  
    
    const converter = new CurrencyConverter();
  
    
    let amount: number = 1;
    let fromCurrency: string = 'USD';
    let toCurrency: string = 'EUR';
    let result: string | null = null; 
    let currencies: {name: string; code: string}[] = converter.getCurrencies(); 
  

    async function handleConvert() {
      try {
        const convertedAmount = converter.convert(amount, fromCurrency, toCurrency);
        const promiseRes = await convertedAmount;
        result = promiseRes.amount.toFixed(2);
      } catch (error) {
        console.error(error);
        result = 'Error: Invalid currency conversion';
      }
    }
  </script>
  
  <div class="p-6 max-w-lg mx-auto rounded-xl shadow-md space-y-6">
    <h1 class="text-2xl font-bold text-center text-gray-800">Currency Converter</h1>
  
    <div class="space-y-4">
      
      <div>
        <label for="amount" class="block text-sm font-medium text-gray-700">Amount</label>
        <input
          id="amount"
          type="number"
          bind:value={amount}
          min="0"
          step="0.01"
          class="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
        />
      </div>
  
      <div>
        <label for="fromCurrency" class="block text-sm font-medium text-gray-700">From</label>
        <select
          id="fromCurrency"
          bind:value={fromCurrency}
          class="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
        >
          {#each currencies as currency}
            <option value={currency.code}>{currency.code} - {currency.name}</option>
          {/each}
        </select>
      </div>
  
      <div>
        <label for="toCurrency" class="block text-sm font-medium text-gray-700">To</label>
        <select
            id="toCurrency"
            bind:value={toCurrency}
            class="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm max-h-60 overflow-y-auto"
            >
            {#each currencies as currency}
                <option value={currency.code}>{currency.code} - {currency.name}</option>
            {/each}
        </select>

      </div>
    </div>
  
    <button
      on:click={handleConvert}
      class="w-full bg-indigo-600 text-white px-4 py-2 rounded-lg shadow-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
    >
      Convert
    </button>

    {#if result !== null}
      <div class="text-center text-lg font-medium text-gray-800">
        {amount} {fromCurrency} = {result} {toCurrency}
      </div>
    {/if}
  </div>
  