<template>
  <div class="login">
    <div class="login-container">
      <p>Login</p>
      <input
        ref="loginInput"
        autocomplete="off"
        type="email"
        name="email"
        class="email"
        id="email"
        placeholder="Email"
        v-on:keypress="onKeyPress"
      >
      <p v-if="isLoginError" class="error-message">Oops, we could not find you. Try again or register.</p>
      <div class="button-container">
        <button type="button" v-on:click="onClickLogin" class="login">Login</button>
        <button v-on:click="openRegistration" class="register-button">Register</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Login",
  props: {
    isLoginError: Boolean
  },
  methods: {
    onKeyPress(e) {
      if (e.key === "Enter") this.$emit("on-login", e.target.value);
    },
    onClickLogin() {
      this.$emit("on-login", this.$refs.loginInput);
    },
    openRegistration() {
      this.$emit("on-registration");
    }
  }
};
</script>
<style lang="scss" src="@/styles/login.scss">
</style>
