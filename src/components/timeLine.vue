<template>
  <v-timeline direction="horizontal">
    <v-timeline-item
      v-for="(paragraphe, i) in paragraphes"
      :key="i"
      dot-color="indigo"
      fill-dot
    >
      <v-card>
        <v-card-text>{{ paragraphe }}</v-card-text>
      </v-card>
    </v-timeline-item>
  </v-timeline>
</template>

<script setup>
const props = defineProps({
  paragraphes: {
    type: Array,
    default: () => [],
  },
});
</script>
