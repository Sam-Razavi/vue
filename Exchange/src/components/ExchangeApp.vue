<template>
    <div class="hero">
      <HeroImage :imageUrl="heroImageUrl" :imageAlt="heroImageAlt" />
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title mb-4">Currency Converter</h5>
                <div class="form-group">
                  <label for="amount">Amount (in {{ currencyType }}):</label>
                  <input
                    type="number"
                    id="amount"
                    class="form-control"
                    v-model="currencyAmount"
                  />
                </div>
                <div class="form-group">
                  <label for="currencyTo">Convert to SEK (in {{ swedishCrowns }}):</label>
                  <select id="currencyTo" class="form-control" v-model="currencyType">
                    <option value="SEK">Swedish Krona (SEK)</option>
                    <option value="USD">US Dollar (USD)</option>
                    <option value="EUR">Euro (EUR)</option>
                  </select>
                </div>
                <div class="form-group">
                  <button class="btn btn-primary" @click="convert">
                    Convert
                  </button>
                </div>
                <div class="form-group" v-if="conversionRate !== null">
                  <label>Conversion rate:</label>
                  <p>1 {{ currencyType }} = {{ conversionRate }} SEK</p>
                </div>
                <div class="form-group" v-if="swedishCrowns !== null">
                  <label>Result:</label>
                  <p>{{ swedishCrowns }} SEK</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <RouterView/>
  </template>

  <script>
  import axios from 'axios';
  import HeroImage from "@/components/HeroImage.vue";
  import { RouterLink, RouterView } from 'vue-router'

  export default {
    components: {
      HeroImage,
    },
    data() {
      return {
        heroImageUrl: ('@/assets/img/hero.jpeg'),
        heroImageAlt: "Currency Exchange",
        currencyAmount: null,
        currencyType: 'EUR',
        conversionRate: null,
        swedishCrowns: null,
        apiKey: 'c80a34a0d72244519a17acc39635a90e',
      };
    },
    methods: {
      async convert() {
        try {
          const response = await axios.get(`https://openexchangerates.org/api/latest.json?app_id=${this.apiKey}&base=${this.currencyType}&symbols=SEK`);
          const conversionRate = response.data.rates.SEK;
          this.conversionRate = conversionRate.toFixed(2);
          this.swedishCrowns = (this.currencyAmount * conversionRate).toFixed(2);
        } catch (error) {
          console.error(error);
        }
      },
    },
  };
  </script>

  <style scoped>
  .hero {
  position: relative;
  height: 100vh;
}

.hero-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
}

.card {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
  width: 80%;
  max-width: 500px;
}
.btn btn-primary hover{
    background-color: red;
}

  </style>
