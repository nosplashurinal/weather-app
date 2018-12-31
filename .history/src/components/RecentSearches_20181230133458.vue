<template>
  <div class="recent-searches">
    <p>Recent Searches</p>
    <ul class="list">
      <li v-for="item in items">{{ item.message }}</li>
    </ul>
  </div>
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


