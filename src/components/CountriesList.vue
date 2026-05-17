<template>
  <div class="container mt-4">
    <div v-if="countries.length === 0">
      <p>Loading countries...</p>
    </div>

    <div v-else class="row">
      <div class="col-md-4">
        <div
          v-for="country in countries"
          :key="country.alpha3Code"
          class="mb-2"
        >
          <router-link
            :to="`/list/${country.alpha3Code}`"
            class="text-decoration-none"
          >
            {{ country.name.common || country.name }}
          </router-link>
        </div>
      </div>

      <div class="col-md-8">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import countriesData from "../countries.json";

const countries = ref([]);

onMounted(() => {
  countries.value = countriesData;
});
</script>