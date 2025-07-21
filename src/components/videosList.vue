<template>
  <v-card class="mx-auto mt-4" min-height="140" rounded="lg">
    <v-list density="compact">
      <v-list-subheader>Vidéos</v-list-subheader>
      <v-list-item
        v-for="(video, i) in videos"
        :key="i"
        :value="video"
        color="primary"
        :href="video.url"
        target="_blank"
      >
        <template v-slot:prepend>
          <v-icon icon="mdi-youtube"></v-icon>
        </template>
        <v-list-item-title>{{ video.name }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script setup>
const props = defineProps({
  videos: {
    type: Array,
    default: () => [],
  },
});
</script>
