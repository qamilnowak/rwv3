<template>
  <p>Register for events here</p>
  <button @click="register">Register Me</button>
</template>
<script>
export default {
  name: 'Register',
  inject: ['GStore'],
  props: ['event'],
  methods: {
    register() {
      this.GStore.flashMessage =
        'You are successfully registered for: ' + this.event.title
      setTimeout(() => {
        this.GStore.flashMessage = ''
      }, 3000)
      this.$router.push({
        name: 'EventDetails',
      })
    },
  },
}
</script>

<style scoped></style>
