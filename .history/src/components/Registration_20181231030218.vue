<template>
  <div class="registration">
    <p>Please take a few moments to register.</p>
    <div class="fields-container">
      <transition name="slide-away">
        <input
          placeholder="Enter your Email"
          autofocus
          autocomplete="off"
          type="email"
          name="email"
          class="email"
          id="email"
        />
        <input
          placeholder="Enter your Name"
          autofocus
          autocomplete="off"
          type="text"
          name="name"
          class="name"
          id="name"
        />
        <input
          placeholder="Mobile"
          autofocus
          autocomplete="off"
          type="phone"
          name="mobile"
          class="mobile"
          id="mobile"
        />
        <input
          placeholder="Where are you located?"
          autofocus
          autocomplete="off"
          type="text"
          name="location"
          class="location"
          id="location"
        />
      </transition>
    </div>
    <button type="button" class="next">Next</button>
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
  methods: {}
};
</script>
<style lang="scss" src="@/styles/registration.scss">
</style>
