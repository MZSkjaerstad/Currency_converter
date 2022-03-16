<template>
   <main class="currency">
      <div class="currency__app">
         <section class="currency__logo">
            <div class="currency__logo--upper">Cash</div>
            <div class="currency__logo--lower">Converter</div>
         </section>

         <section class="currency__controller">
            <input class="currency__input" type="number" placeholder="Type in your value" v-model="input">

            <div class="currency__convert">
               <select class="currency__select" v-model="fromCurrency">
                  <option v-for="currency, index in allCurrencies" :value="index">{{ index }}</option>
               </select>

               <svg class="currency__arrow" width="41" height="40" viewBox="0 0 41 40" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M5.50215 34.3077C5.25607 34.2229 5.04258 34.0635 4.89147 33.8516C4.74035 33.6397 4.65915 33.3859 4.65918 33.1256C4.65918 27.4147 5.80605 22.8475 8.06934 19.5498C10.8037 15.5655 15.117 13.4202 20.9092 13.1537V6.87562C20.9092 6.63086 20.9811 6.39149 21.1159 6.1872C21.2507 5.98292 21.4426 5.82271 21.6676 5.72645C21.8926 5.63019 22.141 5.60212 22.3818 5.64572C22.6227 5.68933 22.8454 5.80268 23.0225 5.97172L36.7725 19.0967C36.8946 19.2134 36.9919 19.3537 37.0584 19.5091C37.1248 19.6644 37.1591 19.8316 37.1591 20.0006C37.1591 20.1696 37.1248 20.3368 37.0584 20.4922C36.9919 20.6475 36.8946 20.7878 36.7725 20.9045L23.0225 34.0295C22.8454 34.1986 22.6227 34.3119 22.3818 34.3555C22.141 34.3991 21.8926 34.3711 21.6676 34.2748C21.4426 34.1785 21.2507 34.0183 21.1159 33.814C20.9811 33.6098 20.9092 33.3704 20.9092 33.1256V26.8936C17.3936 26.9998 14.7373 27.5694 12.6225 28.6623C10.3373 29.8436 8.68027 31.6084 6.89434 33.8967C6.73412 34.1019 6.51385 34.2519 6.26428 34.3258C6.0147 34.3998 5.74826 34.394 5.50215 34.3092V34.3077Z" fill="#90e0ef"/>
               </svg>

               <select class="currency__select" v-model="toCurrency">
                  <option v-for="currency, index in allCurrencies" :value="index">{{ index }}</option>
               </select>
            </div>

            <div class="currency__output">
            {{ output }}
            </div>

            <button class="currency__button" @click="convertTo">Convert</button>
         </section>
      </div>
   </main>
</template>

<script>


export default {

   data() {
      return {
         allCurrencies: {},
         fromCurrency: 'eur',
         toCurrency: 'nok',
         input: null,
         output: 0,
      }
   },

   async created() {
      this.fetchRates();
   },

   methods: {
      async fetchRates() {
         const url = `https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/eur.json`;
         const res =  await fetch(url);
         const results = await res.json();
         this.allCurrencies = results.eur;
         console.log(results)
      },

      /* Old Converter Function
      convertTo() {
         this.output = this.input / this.fromCurrency * this.toCurrency;
      },
      */

      async convertTo() {
         const url = `https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/${this.fromCurrency}/${this.toCurrency}.json`;
         const res =  await fetch(url);
         const results = await res.json();

         console.log(results)

         const output = this.input * results[this.toCurrency]
         this.output = output;
      }
   }
}
</script>

<style>
   .currency {
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: space-evenly;
      align-items: center;
   }

   .currency__app {
      height: 40%;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
   }

   .currency__logo {
      display: flex;
      flex-direction: column;
      align-items: center;
      
   }

   .currency__logo--upper {
      font-family: jaf-lapture;
      font-size: 5rem;
      font-weight: 500;
      letter-spacing: 0.6rem;
      text-transform: lowercase;
   }

   .currency__logo--lower {
      font-size: 1.3rem;
      font-style: italic;
      font-weight: 700;
      text-transform: uppercase;
      transform: translateY(-0.5rem);
   }

   .currency__controller {
      background: var(--highlight-color);
      border: solid var(--secondary-color) 1px;
      border-radius: 15px;
      padding: 1rem;
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .currency__input {
      width: 100%;
      height: 1.6rem;
      color: var(--primary-color);
      background: var(--highlight-color);
      border: solid var(--secondary-color) 1px;
      border-radius: 10px;
      text-align: center;
      margin-bottom: 0.6rem;
   }

   .currency__input:hover {
      color: var(--highlight-color);
      background: var(--primary-color);
   }

   .currency__convert {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
   }

   .currency__select {
      height: 1.6rem;
      color: var(--primary-color);
      background: var(--highlight-color);
      border: solid var(--secondary-color) 1px;
      border-radius: 10px;
      text-align: center;
   }

   .currency__select:hover {
      color: var(--highlight-color);
      background: var(--primary-color);
   }

   .currency__arrow {
      height: 1.2rem;
   }

   .currency__output {
      height: 4rem;
      color: var(--primary-color);
      font-size: 1.4rem;
      font-style: italic;
      font-weight: 500;
      display: flex;
      align-items: center;
   }

   .currency__button {
      width: 100%;
      color: var(--primary-color);
      font-size: 1.2rem;
      font-style: italic;
      font-weight: 600;
      font-family: jaf-lapture;
      text-transform: lowercase;
      background: var(--highlight-color);
      border: solid var(--secondary-color) 1px;
      border-radius: 10px;
      text-align: center;
      padding: 0.4rem;
   }

   .currency__button:hover {
      color: var(--highlight-color);
      background: var(--primary-color);
   }
</style>