<template>
  <div class="login">
    <input
      ref="searchInput"
      autocomplete="off"
      type="email"
      name="email"
      class="email"
      id="search"
      v-model="value"
      v-on:focus="onChange"
      v-on:blur="onBlur"
      v-on:keypress="onKeyPress"
    >
  </div>
</template>
<script>
export default {
  name: "Login"
};
</script>
<style lang="scss" src="@/styles/login.scss">
</style>
