<template>
  <v-combobox
    class="champion-combobox"
    clearable
    v-model="selectedChampion"
    :items="champions"
    :label="label"
  />
</template>

<script setup>
import { ref, watch, defineProps, defineEmits, onMounted } from 'vue';

const props = defineProps({
  modelValue: String,
  label: {
    type: String,
    default: 'Choisir un champion',
  },
});
const emit = defineEmits(['update:modelValue']);

const champions = ref([]);

onMounted(async () => {
  const data = await import('@/assets/data/15.13.1/data/fr_FR/champion.json');
  champions.value = Object.values(data.default.data).map(champ => champ.id);
});

const selectedChampion = ref(props.modelValue ?? '');

watch(() => props.modelValue, (nv) => {
  if (nv !== selectedChampion.value) selectedChampion.value = nv;
});

watch(selectedChampion, (nv) => {
  // Vérifie si la valeur existe dans la liste des champions
  if (champions.value.includes(nv)) {
    emit('update:modelValue', nv);
  } else {
    emit('update:modelValue', ''); // Ou tu peux garder la dernière valeur valide
  }
});
</script>

<style scoped>
.champion-combobox {
  width: 300px;
  max-width: 100%;
}
</style>
