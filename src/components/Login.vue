<template>
  <div class="login">
    <p>Login</p>
    <div class="login-container">
      <input
        ref="loginInput"
        autofocus
        type="email"
        name="email"
        class="email"
        id="email"
        placeholder="Email"
        v-on:keypress="onKeyPress"
      >
      <small v-if="isLoginError" class="error-message">Oops, we could not find you. Try again or
        <span v-on:click="openRegistration">register.</span>
      </small>
      <div class="button-container">
        <button type="button" v-on:click="onClickLogin" class="login-button">Login</button>
        <button v-on:click="openRegistration" class="register-button">Not a user? Register.</button>
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
      this.$emit("on-login", this.$refs.loginInput.value);
    },
    openRegistration() {
      this.$emit("open-registration");
    }
  }
};
</script>
<style lang="scss" src="@/styles/login.scss">
</style>
