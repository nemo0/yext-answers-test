<template>
  <div>
    <universal-autocomplete />
  </div>
  <div class="flex">
    <div class="w-1/2">
      <universal-search />
    </div>
    <div class="w-1/2">
      <vertical-autocomplete />
    </div>
  </div>
</template>

<script>
import UniversalAutocomplete from "./components/UniversalAutocomplete.vue";
import UniversalSearch from "./components/UniversalSearch.vue";
import VerticalAutocomplete from "./components/VerticalAutocomplete.vue";

export default {
  components: {
    UniversalAutocomplete,
    UniversalSearch,
    VerticalAutocomplete,
  },
};
</script>