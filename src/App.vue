<script setup lang="ts">
import PageHeader from './components/PageHeader.vue';
import axiosClient from './Utils/axios';
import { onMounted, ref, watch } from 'vue';
import Country from './models/country.model';
import CountryList from './components/CountryList.vue'


const countries = ref<Country[]>([]);
const search = ref("");
const filteredCountries = ref<Country[]>([]);
const page = ref(1);
const itemsPerPage = ref(12);
const paginatedCountries = ref<Country[]>([]);
const totalItems = ref(0);

const fetchCountries = async () => {
  try {
    const { data } = await axiosClient.get("/all");
    countries.value = data;
    totalItems.value = countries.value.length;
  } catch (error) {
    console.log(error);

  }
}
const sliceCountries = (currentCountries: Country[]) => {
  const start = (page.value - 1) * itemsPerPage.value;
  const end = page.value * itemsPerPage.value;
  paginatedCountries.value = currentCountries.slice(start, end);
}
const filterCountries = () => {
  filteredCountries.value = countries.value.filter((country) => country.name.common.toLowerCase().includes(search.value.toLowerCase()));
}
const changePage = (newPage: number) => {
  page.value = newPage;
}
onMounted(() => {
  fetchCountries();
});

watch([countries, page, filteredCountries], () => {
  sliceCountries(
    filteredCountries.value.length <= 0 && search.value === "" ? countries.value : filteredCountries.value 
    );
});
</script>

<template>  
  <div>
    <PageHeader />
  </div>
  <div class="container max-w-screen-ls mx-auto px-6">
    <div class="mb-8">
      <input type="text" class="border border-gray-400 rounded w-full p-1 px-4" placeholder="Search by country name."
        @input="filterCountries" v-model="search">
    </div>
    <div class="mb-8 flex justify-center space-x-6">
      <button @click="_$event => changePage(page - 1)" :disabled="page <= 1" :class="{'opacity-50': page <= 1 }">Previous
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-widxth="1.5" stroke="currentColor"
          class="w-20 h-20 stroke-cyan-500 hover:stroke-cyan-700">
          <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5L8.25 12l7.5-7.5" />
        </svg>
      </button>
      <button @click="_$event => changePage(page + 1)" :disabled="page >= totalItems / itemsPerPage" :class="{'opacity-50': page >= totalItems / itemsPerPage}"> Next
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
          class="w-20 h-20 stroke-cyan-500 hover:stroke-cyan-700">
          <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
        </svg>

      </button>
    </div>
    <CountryList :countries="paginatedCountries" />
  </div>
</template>