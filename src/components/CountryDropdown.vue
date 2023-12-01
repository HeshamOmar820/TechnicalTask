<!-- src/components/CountryDropdown.vue -->

<template>
    <select class="text-lg text-black pl-2 mb-5 h-8 rounded-md focus:outline-none focus:shadow-outline" v-bind="selectedCountry" required>
        <option value="" disabled selected hidden>Choose a country</option>
        <option v-for="country in countries" :key="country.code" :value="country.name">
            {{ country.name }}
        </option>
    </select>
</template>

<script >
import {countries} from 'i18n-iso-countries/langs/en.json';


export default {
    props: {
        selectedCountry: String,
    },
    data() {
        return {
            countries: {},
        };
    },
    mounted() {
        this.fetchingCountryNames();
    },
    methods: {
        fetchingCountryNames() {
            this.countries = Object.keys(countries).map((key) => ({ key, name: this.modelingCountryName(countries[key])}))
        },
        modelingCountryName(name) { 
            return typeof name === 'string' ? name : name[name.length - 1]
        }
    },
};
</script>
