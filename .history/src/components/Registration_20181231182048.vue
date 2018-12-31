<template>
  <div class="registration">
    <p>Register</p>
    <div class="fields-container">
      <input
        placeholder="Enter your Email"
        autofocus
        autocomplete="off"
        type="email"
        name="email"
        class="email"
        id="email"
        v-model="email"
      >
      <input
        placeholder="Enter your Name"
        autofocus
        autocomplete="off"
        type="text"
        name="name"
        class="name"
        id="name"
        v-model="name"
      >
      <input
        placeholder="Mobile"
        autofocus
        autocomplete="off"
        type="phone"
        name="mobile"
        class="mobile"
        id="mobile"
        v-model="mobile"
      >
      <input
        placeholder="Where are you located?"
        autofocus
        autocomplete="off"
        type="text"
        name="location"
        class="location"
        id="location"
        v-model="location"
      >
    </div>
    <button type="button" class="next" v-on:click="onRegister">Register</button>
  </div>
</template>
<script>
export default {
  name: "Registration",
  data() {
    return {
      email: null,
      name: null,
      mobile: null,
      location: null
    };
  },
  methods: {
    onRegister() {
      const { email, name, mobile, location } = this;
      const userObject = {
        email,
        name,
        mobile,
        location
      };
      window.localStorage.setItem("user", JSON.stringify(userObject));
      this.$emit("on-success");
    }
  }
};
</script>
<style lang="scss" src="@/styles/registration.scss">
</style>
