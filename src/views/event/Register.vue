<template>
  <p>Register for events here</p>
  <button @click="register">Register Me</button>
</template>
<script>
export default {
  name: 'Register',
  props: {
    id: {
      required: true,
    },
  },
  methods: {
    register() {
      this.$router.push({
        name: 'EventDetails'
      })
    },
  },
}
</script>

<style scoped></style>
