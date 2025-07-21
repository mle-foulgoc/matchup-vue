<template>
  <v-app id="inspire">
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="20" md="2">
            <v-sheet min-height="268" rounded="lg">
              <div
                class="mt-4"
                style="
                  display: flex;
                  flex-direction: column;
                  align-items: center;
                "
              >
                <Avatar v-if="monChampion" :champion-id="monChampion" />
                <StatsTable v-if="monChampion" :stats="monChampionStats" />
              </div>
            </v-sheet>
            <v-sheet
              min-height="140"
              rounded="lg"
              class="mt-4 d-flex align-center justify-center"
              v-if="hasNotes"
            >
              <itemsAdvice :items="notesData?.items || []" />
            </v-sheet>
          </v-col>

          <v-col cols="12" md="8">
            <v-sheet rounded="lg">
              <Spells />

              <timeline
                v-if="hasNotes"
                :paragraphes="notesData?.infos?.paragraphes || []"
              />

              <!-- Ajout du lecteur vidéo sous le timeline -->
              <div
                v-if="hasNotes && notesData?.videos && notesData.videos[0]"
                class="mt-4 video-center"
              >
                <div class="video-square">
                  <iframe
                    width="100%"
                    height="100%"
                    :src="youtubeEmbedUrl(notesData.videos[0].url)"
                    frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                    allowfullscreen
                    style="border-radius: 12px"
                  ></iframe>
                </div>
              </div>
            </v-sheet>
          </v-col>

          <v-col cols="12" md="2">
            <v-sheet min-height="268" rounded="lg">
              <div
                class="mt-4"
                style="
                  display: flex;
                  flex-direction: column;
                  align-items: center;
                "
              >
                <Avatar
                  v-if="adversaireChampion"
                  :champion-id="adversaireChampion"
                />
                <StatsTable
                  v-if="adversaireChampion"
                  :stats="adversaireChampionStats"
                />
              </div>
            </v-sheet>
            <videos v-if="hasNotes" :videos="notesData?.videos || []" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";
import { useRoute } from "vue-router";
import Avatar from "@/components/Avatar.vue";
import championFull from "@/assets/data/15.13.1/data/fr_FR/championFull.json";
import StatsTable from "@/components/StatsTable.vue";
import videos from "@/components/videosList.vue";
import timeline from "@/components/timeLine.vue";
import itemsAdvice from "@/components/itemsAdvice.vue";

const route = useRoute();
const monChampion = route.query.monChampion;
const adversaireChampion = route.query.adversaireChampion;
const matchupId = route.query.matchupId;

const monChampionStats = computed(() => {
  const champ = championFull.data[monChampion];
  return champ ? champ.stats : {};
});
const adversaireChampionStats = computed(() => {
  const champ = championFull.data[adversaireChampion];
  return champ ? champ.stats : {};
});

// Ajout pour la condition d'affichage
const hasNotes = ref(false);
const notesData = ref(null);

async function checkNotes() {
  const matchupId = route.query.matchupId;
  if (matchupId) {
    try {
      const res = await fetch(`/notes/${matchupId}.json`);
      if (res.ok) {
        notesData.value = await res.json();
        hasNotes.value = true;
      } else {
        notesData.value = null;
        hasNotes.value = false;
      }
    } catch (e) {
      notesData.value = null;
      hasNotes.value = false;
    }
  } else {
    notesData.value = null;
    hasNotes.value = false;
  }
}

onMounted(checkNotes);
watch(() => route.query.matchupId, checkNotes);

function youtubeEmbedUrl(url) {
  // Extrait l'ID de la vidéo YouTube et construit l'URL d'embed
  const match = url.match(
    /(?:youtu\.be\/|youtube\.com\/(?:watch\?v=|embed\/|v\/))([\w-]{11})/
  );
  return match ? `https://www.youtube.com/embed/${match[1]}` : "";
}
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
.video-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
.video-square {
  width: 90%;
  aspect-ratio: 1 / 1;
  max-width: 600px;
  min-width: 200px;
  background: #000;
  border-radius: 12px;
  overflow: hidden;
  display: flex;
}
</style>
