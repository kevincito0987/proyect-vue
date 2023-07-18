<script setup lang="ts">
import PageHeader from './components/PageHeader.vue';
import axiosClient from './Utils/axios';
import { onMounted, ref } from 'vue';
import { Country } from './models/country.model';
import CountryList from './components/CountryList.vue'


const countries = ref<Country[]>([]);
const fetchCountries = async () => {
  try {
    const { data } = await axiosClient.get("/all"); 
    countries.value = data;
  } catch (error) {
    console.log(error);
    
  }
}
onMounted(() => {
  fetchCountries();
})
</script>

<template>
  <div>
    <PageHeader/>
  </div>
  <div class="container max-w-screen-ls mx-auto px-6">
    <CountryList :countries="countries"/>
  </div>
</template>