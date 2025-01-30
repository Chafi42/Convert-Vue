<template>
    <div class="wrapper">
      
      <input 
        type="number" 
        min="0" 
        class="amount" 
        v-model="amount" 
        placeholder="Amount to convert"
      />
      
      <ul class="currencies" @click="toggleCurrencyList">
        <li 
          v-for="currency in currencies" 
          :key="currency.code" 
          :class="{ active: selectedCurrency === currency.code }"
          @click="selectCurrency(currency.code)"
        >
          {{ currency.name }} ({{ currency.code }})
        </li>
      </ul>
  
      <ul class="results">
        <li 
          v-for="currency in currencies" 
          :key="currency.code"
        >
          <span class="name">{{ currency.name }}</span>
          <span class="iso">{{ currency.code }}</span>
          <span class="result">{{ convertCurrency(currency.code).toFixed(2) }}</span>
        </li>
      </ul>
  
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        amount: 0,
        selectedCurrency: "EUR",
        exchangeRates: {},
        currencies: [
          { code: "EUR", name: "Euro" },
          { code: "USD", name: "USDollar" },
          { code: "GBP", name: "Pound" },
          { code: "JPY", name: "Yen" },
          { code: "AUD", name: "Australian Dollar" },
          { code: "CHF", name: "Franc" },
          { code: "ZAR", name: "Rand" },
          { code: "CAD", name: "Canadian Dollar" }
        ],
      };
    },
    methods: {
      async fetchExchangeRates() {
        try {
          const response = await fetch("https://www.floatrates.com/daily/eur.json");
          this.exchangeRates = await response.json();
        } catch (error) {
          console.error("Error fetching exchange rates:", error);
        }
      },
      selectCurrency(code) {
        this.selectedCurrency = code;
      },
      convertCurrency(targetCurrency) {
        if (!this.exchangeRates[targetCurrency.toLowerCase()] || this.selectedCurrency === targetCurrency) {
          return this.amount;
        }
        const rate = this.exchangeRates[targetCurrency.toLowerCase()].rate;
        return this.amount * rate;
      },
      toggleCurrencyList(event) {
        event.currentTarget.classList.toggle("open");
      }
    },
    mounted() {
      this.fetchExchangeRates();
    }
  };

  
  </script>
  
<style scoped>

  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro:300');

body {
  background: #222;
  font-family: 'Source Sans Pro', Helvetica, Arial, sans-serif;
  overflow: hidden;
  margin: 0;
}

.wrapper {
  width: 650px;
  margin: 20px auto;
  position: relative;
}

.instructions {
  width: 100%;
  height: 40px;
  font: 18px/40px 'Source Sans Pro', Helvetica, Arial, sans-serif;
  color: #4597d1;
  text-align: center;
}

input {
  width: 385px;
  height: 50px;
  font-size: 30px;
  float: left;
  border: none;
  background: rgba(255, 255, 255, 0.1);
  padding-left: 20px;
  margin-right: 20px;
  font-family: 'Source Sans Pro', Helvetica, Arial, sans-serif;
  color: #8ec2da;
  box-sizing: border-box;
}

input:focus {
  border: 1px solid #f1e85b;
  outline: none;
}

.currencies {
  list-style: none;
  margin: 0;
  padding: 0;
  font-size: 24px;
  position: absolute;
  right: 0;
  max-height: 50px;
  overflow: hidden;
  transition: all 0.5s ease-in-out;
  z-index: 10;
}

.currencies li {
  height: 40px;
  background: #4162a9;
  margin-bottom: 5px;
  line-height: 40px;
  text-align: center;
  padding: 5px;
  color: #f5fcd0;
  cursor: pointer;
}

.currencies li:hover {
  background: #ee3b37;
}

.open {
  max-height: 600px;
}

.results {
  float: left;
  width: 100%;
  list-style: none;
  margin: 0;
  padding: 0;
}

.results li {
  width: 100%;
  height: 30px;
  line-height: 30px;
  background: rgba(0, 0, 0, 0.2);
  margin-top: 10px;
  font-size: 25px;
  color: #f5fcd0;
}

.name,
.iso,
.result {
  width: 33.33%;
  float: left;
  text-align: center;
}

.hide {
  display: none;
}

.warning {
  clear: both;
  color: #f8f8f8;
  font-size: 13px;
  padding: 5px 0;
}

.yahoo {
  text-align: center;
}
</style>
  