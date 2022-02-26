<template>
  <div
    class="
      bg-gray-50
      min-w-screen min-h-screen
      flex
      justify-center
      items-center
    "
  >
    <div class="max-w-xs relative space-y-3">
      <label for="search" class="text-gray-900">
        Type the name of a country to search
      </label>

      <input
        type="text"
        id="search"
        v-model="searchTerm"
        placeholder="Type here..."
        class="p-3 mb-0.5 w-full border border-gray-300 rounded"
      />

      <ul
        v-if="searchCountries.length"
        class="
          w-full
          rounded
          bg-white
          border border-gray-300
          px-4
          py-2
          space-y-1
          absolute
          z-10
        "
      >
        <li class="px-1 pt-1 pb-2 font-bold border-b border-gray-200">
          Showing {{ searchCountries.length }} of {{ countries.length }} results
        </li>
        <li
          v-for="country in searchCountries"
          :key="country.id"
          @click="selectCountry(country.id)"
          class="cursor-pointer hover:bg-gray-100 p-1"
        >
          {{ country.name }}
        </li>
      </ul>
      <p>{{ searchCountries[0] }}</p>
      <p v-if="selectedCountry" class="text-lg pt-2 absolute">
        You have selected:
        <span class="font-semibold">{{ selectedCountry }}</span>
      </p>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { provideCore } from "@yext/answers-core";

const core = provideCore({
  apiKey: import.meta.env.VITE_YEXT_API,
  experienceKey: import.meta.env.VITE_YEXT_EXP,
  locale: "en",
  experienceVersion: "PRODUCTION",
});

console.log(import.meta.env);

export default {
  setup() {
    let searchTerm = ref("");

    const searchCountries = computed(() => {
      if (searchTerm.value === "") {
        return [];
      }

      return core
        .universalSearch({
          query: searchTerm.value,
        })
        .then((response) => {
          console.log(response.verticalResults[0].results);
          return response.verticalResults[0].results;
        });
    });
    console.log(searchCountries);
    const selectCountry = (country) => {
      selectedCountry.value = country;
      searchTerm.value = "";
    };

    let selectedCountry = ref("");

    return {
      searchTerm,
      searchCountries,
      selectCountry,
      selectedCountry,
    };
  },
};
</script>
