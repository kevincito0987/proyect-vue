<script setup lang="ts">
import Country from '../models/country.model';
import CountryItem from './Countrytem.vue'

interface Props {
    countries: Country[];
}
defineProps<Props>();
</script>
<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
        <div class="border border-gray-400 rounded p-3" v-for="country in countries">
           <CountryItem :country="country"/> 
        </div>
    </div>
</template>