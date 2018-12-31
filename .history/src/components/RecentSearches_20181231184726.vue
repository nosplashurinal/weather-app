<template>
  <div class="recent-searches">
    <p>Recent Searches</p>
    <ul class="list" v-if="items.length > 0">
      <li :key="item.result.id" v-for="item in items" v-on:click="onClick(item.query)">
        <span>{{ item.query }}</span>
        <span>{{`${Math.round(item.result.temp) }&deg;C`}}</span>
      </li>
    </ul>
    <small v-else>Nothing to show.<small>
  </div>
</template>
<script>
export default {
  name: "RecentSearches",
  props: {
    onClick: Function
  },
  data() {
    return {
      items: JSON.parse(window.localStorage.getItem("searches"))
    };
  }
};
</script>
<style lang="scss" src="@/styles/recentSearches.scss">
</style>
