<template>
  <div
    style="display: flex; flex-direction: row; gap: 8px; align-items: center"
  >
    <v-img
      v-for="id in items"
      :key="id"
      width="82"
      height="82"
      cover
      :src="`/public/img/item/${id}.png`"
    />
  </div>
</template>

<script setup>
const props = defineProps({
  items: {
    type: Array,
    default: () => [],
  },
});
</script>
