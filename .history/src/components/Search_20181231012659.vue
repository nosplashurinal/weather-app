<template>
  <div
    class="search-popup"
    v-bind:class="{loading: showLoading, active: inputActive, dirty: isDirty}"
  >
    <input
      ref="searchInput"
      autocomplete="off"
      ref:input
      type="text"
      name="search"
      class="search"
      id="search"
      v-model="value"
      v-on:focus="inputActive = true"
      v-on:blur="onBlur"
      v-on:keypress="onKeyPress"
    >
    <label>Search by City</label>
    <div class="underline"></div>
    <transition
      name="fade"
    ><RecentSearches v-if="inputActive && this.hasRecentSearches()" v-bind:onClick="onClickRecentQuery"/></transition>
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
  computed: {
    hasRecentSearches: function() {
      return !!window.localStorage.getItem("searches");
    }
  },
  methods: {
    onClickRecentQuery(query) {
      this.$emit("on-key-press", query);
      this.value = query;
    },
    onKeyPress(e) {
      if (e.key === "Enter") {
        this.$refs.searchInput.blur();
        this.$emit("on-key-press", e.target.value);
      }
    },
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
