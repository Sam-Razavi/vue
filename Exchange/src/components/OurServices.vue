<template>
    <div class="container">
        <h3 class="text-center my-3">Our Currency List</h3>
        <div class="row justify-content-center">
            <div class="col-md-8">
                <ul class="list-group">
                    <li
                        class="list-group-item"
                        v-for="(currency, index) in displayedCurrencies"
                        :key="index"
                    >
                        {{ currency.name }} - {{ currency.code }}
                    </li>
                </ul>
                <button
                    class="btn btn-primary mt-3"
                    v-if="!showAll"
                    @click="showAllCurrencies"
                >
                    Show All
                </button>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        data() {
            return {
                apiKey: 'c80a34a0d72244519a17acc39635a90e',
                currencies: [],
                displayedCurrencies: [],
                showAll: false
            }
        },
        async created() {
            try {
                const response = await axios.get(
                    `https://openexchangerates.org/api/currencies.json?app_id=${this.apiKey}`
                )
                const currencies = Object.entries(response.data).map(
                    ([code, name]) => ({ code, name })
                )
                this.currencies = currencies
                this.displayedCurrencies = currencies.slice(0, 10)
            } catch (error) {
                console.error(error)
            }
        },
        methods: {
            showAllCurrencies() {
                this.showAll = true
                this.displayedCurrencies = this.currencies
            }
        }
    }
</script>

<style scoped>
    .container {
        width: 60%;
        margin-top: 50%;

        justify-content: center;
    }
</style>
