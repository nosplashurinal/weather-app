<template>
  <div class="login">
    <p>Login</p>
    <input
      autocomplete="off"
      type="email"
      name="email"
      class="email"
      id="email"
      placeholder="Email"
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
