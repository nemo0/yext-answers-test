<template>
  <div class="bg-gray-50 min-w-screen min-h-screen flex justify-center py-10">
    <div class="max-w-xs relative space-y-3 text-center">
      <h3 class="font-extrabold text-lg">Vertical Autocomplete</h3>
      <label for="search" class="text-gray-900">
        Type the name of a data to search(example: "how")
      </label>

      <input
        type="text"
        id="search"
        v-model="searchTerm"
        placeholder="Type here..."
        class="p-3 mb-0.5 w-full border border-gray-300 rounded"
      />
      <div v-if="selectedResult" class="text-lg pt-2">
        You have selected: <br />
        <p class="font-semibold">{{ selectedResult }}</p>
      </div>
      <ul
        v-if="result.results !== undefined && result.results.length"
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
          drop-shadow-md
        "
      >
        <li class="px-1 pt-1 pb-2 font-bold border-b border-gray-200">
          Showing {{ result.results.length }} results
        </li>
        <li
          v-for="r in result.results"
          :key="r.id"
          @click="selectResult(r.value)"
          class="cursor-pointer hover:bg-gray-100 p-1"
        >
          {{ r.value }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, watchEffect } from "vue";
import { provideCore } from "@yext/answers-core";

const core = provideCore({
  apiKey: import.meta.env.VITE_YEXT_API,
  experienceKey: import.meta.env.VITE_YEXT_EXP,
  locale: "en",
  experienceVersion: "PRODUCTION",
});

export default {
  setup() {
    let searchTerm = ref("");
    let result = ref([]);

    const searchResults = watchEffect(async () => {
      if (searchTerm.value === "") {
        return [];
      }
      result.value = await core.verticalAutocomplete({
        input: searchTerm.value,
        verticalKey: "drinks",
        filter: { $eq: "Vodka Martini" },
      });
    });
    const selectResult = (result) => {
      selectedResult.value = result;
      searchTerm.value = "";
    };
    let selectedResult = ref("");

    return {
      searchTerm,
      result,
      searchResults,
      selectResult,
      selectedResult,
    };
  },
};
</script>
