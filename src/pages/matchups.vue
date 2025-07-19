<template>
  <v-app id="inspire">
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="20" md="2">
            <v-sheet min-height="268" rounded="lg">
              <div class="d-flex justify-center mt-4">
                <Avatar v-if="monChampion" :champion-id="monChampion" />
              </div>
            </v-sheet>
          </v-col>

          <v-col cols="12" md="8">
            <v-sheet min-height="40vh" rounded="lg">
              <Spells />
            </v-sheet>
          </v-col>

          <v-col cols="12" md="2">
            <v-sheet min-height="268" rounded="lg">
              <div class="d-flex justify-center mt-4">
                <Avatar v-if="adversaireChampion" :champion-id="adversaireChampion" />
              </div>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { useRoute } from 'vue-router';
import Avatar from "@/components/Avatar.vue";

const route = useRoute();
const monChampion = route.query.monChampion;
const adversaireChampion = route.query.adversaireChampion;
</script>
<style scoped>

.responsive-avatar {
  width: calc((100vw - 200px) / 12) !important;
  height: calc((100vw - 200px) / 12) !important;
  min-width: 40px;
  min-height: 40px;
  max-width: 80px;
  max-height: 80px;
}

/* Responsive breakpoints */
@media (max-width: 768px) {
  .responsive-avatar {
    width: calc((100vw - 100px) / 8) !important;
    height: calc((100vw - 100px) / 8) !important;
  }
}

@media (min-width: 1200px) {
  .responsive-avatar {
    width: calc((100vw - 300px) / 15) !important;
    height: calc((100vw - 300px) / 15) !important;
    max-width: 100px;
    max-height: 100px;
  }
}

</style>
