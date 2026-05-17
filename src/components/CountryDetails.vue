<template>
  <div v-if="country">
    <h2>{{ country.name?.common || country.name }}</h2>

    <img
      :src="flagUrl"
      alt="flag"
      class="mb-3"
    />

    <p>
      <strong>Capital:</strong>
      {{ country.capital?.[0] || country.capital || "N/A" }}
    </p>

    <p>
      <strong>Area:</strong>
      {{ country.area || "N/A" }}
    </p>

    <p>
      <strong>Borders:</strong>
      {{ country.borders?.join(", ") || "None" }}
    </p>
  </div>

  <div v-else>
    <p>Select a country from the list.</p>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";
import { ref, computed, watch } from "vue";
import countriesData from "../countries.json";

const route = useRoute();
const country = ref(null);

const flagUrl = computed(() => {
  const code = country.value?.alpha2Code?.toLowerCase();

  if (!code) {
    return "";
  }

  return `https://flagpedia.net/data/flags/icon/72x54/${code}.png`;
});

function findCountry(code) {
  country.value = countriesData.find((singleCountry) => {
    return singleCountry.alpha3Code === code;
  });
}

watch(
  () => route.params.alpha3Code,
  (newCode) => {
    if (newCode) {
      findCountry(newCode);
    }
  },
  { immediate: true }
);
</script>