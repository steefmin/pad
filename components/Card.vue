<template>
  <div class="column">
    <div class="card">
      <div class="card-content">
        {{ content }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    content: {
      type: String,
      required: true
    },
  }
}
</script>

<style scoped>

.card-content {
  font-family: monospace;
}

</style>
