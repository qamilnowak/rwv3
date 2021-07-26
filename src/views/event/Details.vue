<template>
    <h1>{{ event.title }}</h1>
    <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
    <p>{{ event.description }}</p>
</template>

<script>
export default {
  name: 'EventDetails',
  props: {
    event: {
      required: true,
      type: Object,
    },
  }
}
</script>

<style scoped></style>
