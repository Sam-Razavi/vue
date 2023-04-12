<template>
    <div class="hero">
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-md-6">
                    <div class="card">
                        <div class="card-body">
                            <h5 class="card-title mb-4">Currency Converter</h5>
                            <div class="form-group">
                                <label for="amount"
                                    >Amount (in {{ currencyType }}):</label
                                >
                                <input
                                    type="number"
                                    id="amount"
                                    class="form-control"
                                    v-model="currencyAmount"
                                />
                            </div>
                            <div class="form-group">
                                <label for="currencyTo"
                                    >Convert to SEK (in
                                    {{ swedishCrowns }}):</label
                                >
                                <select
                                    id="currencyTo"
                                    class="form-control"
                                    v-model="currencyType"
                                    v-on:change="convert"
                                >
                                    <option value="USD">US Dollar (USD)</option>
                                    <option value="EUR">Euro (EUR)</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <button
                                    class="btn btn-primary"
                                    @click="convert"
                                    v-if="currencyAmount"
                                >
                                    Convert
                                </button>
                            </div>
                            <div
                                class="form-group"
                                v-if="conversionRate !== null"
                            >
                                <label>Conversion Rate:</label>
                                <p>
                                    1 {{ currencyType }} =
                                    {{ conversionRate }} SEK
                                </p>
                            </div>
                            <div
                                class="form-group"
                                v-if="swedishCrowns !== null"
                            >
                                <label>Result:</label>
                                <p>{{ swedishCrowns }} SEK</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        data() {
            return {
                currencyAmount: null,
                currencyType: 'EUR',
                conversionRate: null,
                swedishCrowns: null,
                apiKey: 'c80a34a0d72244519a17acc39635a90e'
            }
        },
        watch: {
            currencyAmount: function () {
                this.convert()
            },
            currencyType: function () {
                this.convert()
            }
        },
        methods: {
            async convert() {
                try {
                    const response = await axios.get(
                        `https://openexchangerates.org/api/latest.json?app_id=${this.apiKey}&base=${this.currencyType}&symbols=SEK`
                    )
                    const conversionRate = response.data.rates.SEK
                    this.conversionRate = conversionRate.toFixed(2)
                    this.swedishCrowns = (
                        this.currencyAmount * conversionRate
                    ).toFixed(2)
                    console.log('converted')
                } catch (error) {
                    console.error(error)
                }
            }
        }
    }
</script>

<style scoped>
    .card {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 1;
        width: 80%;
        max-width: 500px;
    }
</style>
