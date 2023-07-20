<script setup lang="ts">
import Country  from '../models/country.model.ts';

interface Props {
    country: Country;
}
defineProps<Props>();
</script>
<template>
    <div class="h-56 sm:h-56">
        <img :src="country.flags.png" :alt="country.flags.alt" class="h-56 sm:h-56 w-full">
    </div>
        <h4 class="text-xl font-bold text-gray-600">{{ country.name.common }}</h4>
        <p class="text-sm text-gray-450">Capital: <span class="font-bold"  v-for="capital in country.capital">{{ capital }} <template>, </template></span></p>
        <p class="text-sm text-gray-450">Population: <span class="font-bold">{{ country.population?.toLocaleString() }}</span></p>
        <p class="text-sm text-gray-450">Region: <span class="font-bold">{{ country.region }}</span></p>

</template>