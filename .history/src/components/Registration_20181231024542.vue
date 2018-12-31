<template>
  <div class="registration">
    <p>Register</p>
    <div class="fields-container">
      <transition name="slide-away">
        <input placeholder="Email" autocomplete="off" type="email" name="email" class="email" id="email">
      </transition>
    </div>
  </div>
</template>
<script>
export default {
  name: "Registration",
  methods: {}
};
</script>
<style lang="scss" src="@/styles/registration.scss">
</style>
