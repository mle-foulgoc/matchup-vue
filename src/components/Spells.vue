<template>
  <v-row justify="center" align="center">
    <v-col cols="auto">
      <SpellList
        :championName="monChampion"
        :spells="spellsMonChampion"
        :isMonChampion="true"
      />
    </v-col>
    <v-col cols="auto">
      <SpellList
        :championName="adversaireChampion"
        :spells="spellsAdversaireChampion"
      />
    </v-col>
  </v-row>
</template>

<script setup>
import { useRoute } from "vue-router";
import SpellList from "./SpellList.vue";

const route = useRoute();
const monChampion = computed(() => route.query.monChampion);
const adversaireChampion = computed(() => route.query.adversaireChampion);

const championFull = ref(null);
onMounted(async () => {
  const data = await import(
    "@/assets/data/15.13.1/data/fr_FR/championFull.json"
  );
  championFull.value = data.default.data;
});

function initSpellsForChampion(championName) {
  if (!championFull.value || !championName) return [];
  // On clone les sorts pour leur ajouter la gestion du niveau localement
  const spells =
    championFull.value[championName]?.spells?.map((spell, idx) => ({
      ...spell,
      key: ["Q", "W", "E", "R"][idx],
      level: 0,
      maxLevel: spell.maxrank ?? (idx === 3 ? 3 : 5),
      baseDamage: spell.cooldown?.[0] || 0, // à adapter selon la vraie stat de dégâts si besoin
      damagePerLevel: 0, // à adapter si besoin
      isUpgrading: false,
    })) ?? [];
  return spells;
}

const spellsMonChampion = ref([]);
const spellsAdversaireChampion = ref([]);

watchEffect(() => {
  spellsMonChampion.value = initSpellsForChampion(monChampion.value);
  spellsAdversaireChampion.value = initSpellsForChampion(
    adversaireChampion.value
  );
});

function getSpellIcon(spell) {
  const url = `/img/spell/${spell.image.full}`;
  console.log("Lien image sort:", url, "pour", spell.name, spell.image.full);
  return url;
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
</style>
