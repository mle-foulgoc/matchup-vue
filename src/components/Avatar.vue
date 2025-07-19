<template>
  <v-avatar :image="imageUrl" size="100"></v-avatar>
</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
  championId: {
    type: String,
    required: true
  }
});

const imageUrl = computed(() => `/img/champion/${props.championId}.png`);
</script>