<template>
  <div class="recent-searches">
    <p>Recent Searches</p>
    <ul class="list">
      <li :key="item.result.id" v-for="item in items">{{ item.query }}</li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "RecentSearches",
  data() {
    return {
      items: JSON.parse(window.localStorage.getItem("searches"))
    };
  }
};
</script>
<style lang="scss" src="@/styles/recentSearches.scss">
</style>
