<script setup>
defineProps({
  text: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="c-h1">
    <h1>{{ text }}</h1>
  </div>
</template>

<style scoped lang="scss">
.c-h1 {
  margin: 48px 0 32px;

  h1 {
    font-size: 2rem;
    font-weight: bold;
    line-height: 1.5;
  }
}
</style>