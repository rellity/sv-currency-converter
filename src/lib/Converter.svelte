<script lang="ts">
    import { CurrencyConverter } from "../class/cc";
    import { format } from "date-fns";
    import { toZonedTime } from "date-fns-tz";

    const converter = new CurrencyConverter();
    const timezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
  
    
    let amount: number = 1;
    let amt2: number | null;
    let fromCurrency: string = 'USD';
    let toCurrency: string = 'EUR';
    let result: string | null = null; 
    let currencies: {name: string; code: string}[] = converter.getCurrencies();
    
    let timestamp: string;
    let zoneddate: Date; 
  
    async function handleConvert() {
      try {
        amt2 = amount;
        const convertedAmount = converter.convert(amount, fromCurrency, toCurrency);
        const promiseRes = await convertedAmount;
        result = promiseRes.amount.toFixed(2);
        timestamp = promiseRes.timestamp;
        zoneddate = toZonedTime(timestamp, timezone);
      } catch (error) {
        console.error(error);
        result = 'Error: Invalid currency conversion';
      }
    }
  </script>
  
  <div class="p-6 max-w-lg mx-auto rounded-xl shadow-md space-y-6 bg-white">
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
          onchange="{amt2 = null}"
        />
      </div>
  
      <div>
        <label for="fromCurrency" class="block text-sm font-medium text-gray-700">From</label>
        <select
          id="fromCurrency"
          bind:value={fromCurrency}
          class="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
          onchange="{result = null}"
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
            onchange="{result = null}"
        >
            {#each currencies as currency}
                <option value={currency.code}>{currency.code} - {currency.name}</option>
            {/each}
        </select>

      </div>
    </div>
  
    <button
      onclick={handleConvert}
      class="w-full bg-indigo-600 text-white px-4 py-2 rounded-lg shadow-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
    >
      Convert
    </button>

    {#if result !== null && amt2 !== null}
      <div class="text-center text-base font-light text-gray-800">
        {amt2} {fromCurrency} = {result} {toCurrency}
        <br />
        as of {format(timestamp, "yyyy-MM-dd HH:mm:ss a")}
      </div>
    {/if}
  </div>
  