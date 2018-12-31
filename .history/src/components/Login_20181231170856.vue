<template>
  <div class="login">
    <div class="login-container">
      <p>Login</p>
      <div class="button-container">
        <button class="login" v-on:click="onLogin">Login</button>
        <button class="register-button">Register</button>
      </div>
    </div>
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
