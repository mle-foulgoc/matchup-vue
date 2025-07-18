<template>
  <v-card class="spell-container" elevation="0" color="transparent">
    <h3 v-if="championName">{{ championName }}</h3>
    <v-row justify="center" no-gutters>
      <v-col
        v-for="(spell, index) in spells"
        :key="index"
        cols="auto"
        class="mx-1"
      >
        <div class="spell-slot">
          <div :class="['spell-icon', spell.key?.toLowerCase?.()]">
            <img
              :src="getSpellIcon(spell)"
              :alt="spell.name"
              class="spell-icon-img"
            />
            <div class="spell-key">{{ spell.key }}</div>
          </div>
        </div>
      </v-col>
    </v-row>
  </v-card>
</template>

<script setup>
const props = defineProps({
  championName: String,
  spells: Array,
});
function getSpellIcon(spell) {
  return `/img/spell/${spell.image.full}`;
}
</script>
