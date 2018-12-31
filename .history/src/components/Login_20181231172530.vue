<template>
  <div class="login">
    <div class="login-container">
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
      <div class="button-container">
        <button v-on:click="$emit("on-login")" class="login">Login</button>
        <button :click="$emit("open-registration")" class="register-button">Register</button>
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
