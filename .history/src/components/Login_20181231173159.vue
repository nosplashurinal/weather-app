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
        <button v-on:click="onClickLogin" class="login">Login</button>
        <button v-on:click="openRegistration" class="register-button">Register</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Login",
  methods: {
    onKeyPress(e) {
      if (e.key === "Enter") this.$emit("on-login");
    },
    onClickLogin() {
      this.$emit("on-login");
    },
    openRegistration() {
      this.$emit("on-registration");
    }
  }
};
</script>
<style lang="scss" src="@/styles/login.scss">
</style>
