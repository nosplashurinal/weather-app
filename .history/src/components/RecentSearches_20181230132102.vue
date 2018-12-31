<template>
  <ul id="recent-searches">
    <li v-for="item in items">{{ item.message }}</li>
  </ul>
</template>
<script>
export default {};
</script>
<style lang="scss" src="@/styles/recentSearches.scss">
</style>


