<template>
  <div
    class="search-popup"
    v-bind:class="{loading: showLoading, active: inputActive, dirty: isDirty}"
  >
    <input
      autocomplete="off"
      ref:input
      type="text"
      name="search"
      class="search"
      id="search"
      v-model="value"
      v-on:focus="inputActive = true"
      v-on:blur="onBlur"
      v-on:keypress="$emit('on-key-press', $event)"
    >
    <label>Search by City</label>
    <div class="underline"></div>
    <transition name="fade">
      <RecentSearches v-show="inputActive" />
    </transition>
  </div>
</template>
<script>
import RecentSearches from "./RecentSearches.vue";

export default {
  name: "Search",
  data() {
    return {
      isDirty: false,
      inputActive: false,
      value: null
    };
  },
  props: {
    showLoading: Boolean
  },
  components: {
    RecentSearches
  },
  methods: {
    onBlur() {
      this.inputActive = false;
      if (this.value && this.value.length !== 0) {
        this.isDirty = true;
      } else {
        this.isDirty = false;
      }
    }
  }
};
</script>
<style lang="scss" src="@/styles/search.scss">
</style>
