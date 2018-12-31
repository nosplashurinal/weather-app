<template>
  <div class="login">
    <input
      autocomplete="off"
      type="email"
      name="email"
      class="email"
      id="email"
      v-on:keypress="onKeyPress"
    >
  </div>
</template>
<script>
export default {
  name: "Login",
  methods: {
    onKeyPress(e) {
      this.$emit("on-login", e.target.value);
    }
  }
};
</script>
<style lang="scss" src="@/styles/login.scss">
</style>
