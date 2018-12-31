<template>
  <ul id="recent-searches">
    <li v-for="item in items">{{ item.message }}</li>
  </ul>
</template>
<script>
export default {
  name: 'RecentSearches',
  data() {
    return {
      items: 
    }
  }
};
</script>
<style lang="scss" src="@/styles/recentSearches.scss">
</style>


