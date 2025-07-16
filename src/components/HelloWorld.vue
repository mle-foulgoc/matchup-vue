<template>
  <v-layout>

    <v-main :min-height="$vuetify.display.mdAndUp ? 800 : 550">
      <v-container class="h-100 d-flex align-center justify-center">
        <div class="w-100 w-md-50 text-center">
          <h1 class="text-h4 text-md-h2 font-weight-bold my-6">
            Affrontez tous les champions
          </h1>

          <div class="text-body-1 text-medium-emphasis mb-10">
            Selectionner un matchup pour comparer les champions et decouvrir vos
            forces et faiblesses
          </div>

          <div class="d-flex ga-4 justify-center">
            <SelectChamp
              v-model="monChampion"
              label="Mon champion"
            />
            <SelectChamp
              v-model="adversaireChampion"
              label="Champion adverse"
            />
          </div>
          <v-btn size="x-large" append-icon="mdi-compare "
            @click="handleCompare"
          >
            {{
              monChampion && adversaireChampion
                ? `Comparer ${monChampion} vs ${adversaireChampion}`
                : 'Comparer'
            }}
          </v-btn>
        </div>

        <div class="v-bg position-absolute top-0 right-0 left-0 bottom-0">
          <div
            aria-hidden="true"
            class="overflow-hidden opacity-20 w-100 h-100"
          />
        </div>
      </v-container>
    </v-main>
  </v-layout>
</template>

<script setup>
import { shallowRef, ref } from "vue";
import { useRouter } from 'vue-router';
import SelectChamp from "./SelectChamp.vue";

const router = useRouter();

const drawer = shallowRef(false);

const monChampion = ref("");
const adversaireChampion = ref("");

const items = [
  { title: "Home", to: "/" },
  { title: "Matchups", to: "/matchups" },
  { title: "Items", to: "/items" },
];

function handleCompare() {
  if (!monChampion.value || !adversaireChampion.value) return;
  // Générer un matchupId unique et cohérent
  const [champ1, champ2] = [monChampion.value, adversaireChampion.value].sort();
  const matchupId = `${champ1}_${champ2}`;
  router.push({
    path: '/matchups',
    query: {
      monChampion: monChampion.value,
      adversaireChampion: adversaireChampion.value,
      matchupId
    }
  });
}
</script>

<style scoped>
.v-bg {
  filter: blur(56px);
  pointer-events: none;
}

.v-bg > div {
  background: linear-gradient(
    to bottom right,
    rgb(var(--v-theme-primary)),
    rgb(var(--v-theme-error))
  );
  z-index: -10;
  clip-path: polygon(
    20% 50%,
    27% 66%,
    41% 66%,
    50% 50%,
    41% 34%,
    27% 34%,
    20% 50%,
    55% 50%,
    62% 66%,
    76% 66%,
    85% 50%,
    76% 34%,
    62% 34%,
    55% 50%,
    30% 50%,
    37% 66%,
    51% 66%,
    60% 50%,
    51% 34%,
    37% 34%,
    30% 50%
  );
}
</style>
