<template>
  <v-card class="spell-container" elevation="0" color="transparent">
    <h3 v-if="championName">{{ championName }}</h3>
    <v-row  justify="center" no-gutters>
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
            <div class="spell-key d-flex align-center justify-center" style="gap: 6px;">
              <span>{{ spell.key }}</span>
              <span v-if="spell.cooldown[0]" class="cooldown-burn">{{ spell.cooldown[0] }}</span>
            </div>
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

<style scoped>
.spell-container {
  backdrop-filter: blur(10px);
  padding: 20px;
}
.spell-icon-img {
  width: 64px;
  height: 64px;
  border-radius: 8px;
  border: 2px solid #444;
  object-fit: cover;
  margin-bottom: 4px;
}
.spell-key {
  font-weight: bold;
  font-size: 1.1em;
  margin-top: 2px;
}
.cooldown-burn {
  font-size: 0.95em;
  color: #888;
  background: rgba(0,0,0,0.08);
  border-radius: 4px;
  padding: 0 4px;
}
.spell-slot {
  margin-bottom: 8px;
}
</style>
