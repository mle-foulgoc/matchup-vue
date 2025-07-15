<template>
  <v-layout>
    <v-navigation-drawer
      v-model="drawer"
      color="primary"
      disable-resize-watcher
    >
      <v-list nav>
        <v-list-item v-for="item in items" :key="item" link :title="item" />
      </v-list>
    </v-navigation-drawer>

    <v-app-bar color="transparent" flat>
      <template #prepend>
        <v-app-bar-nav-icon
          v-if="$vuetify.display.smAndDown"
          @click="drawer = !drawer"
        />
      </template>

      <div class="d-flex flex-1-1-0 ps-md-4">
        <v-avatar image="https://vuetifyjs.b-cdn.net/docs/images/logos/v.png" />
      </div>

      <div class="d-md-flex d-none ga-4 mx-auto">
        <v-btn
          v-for="item in items"
          :key="item"
          class="text-none"
          :text="item"
        />
      </div>

      <div class="d-flex flex-1-1-0 pe-3">
        <v-btn
          append-icon="mdi-chevron-right"
          class="ms-auto text-none"
          slim
          text="Login"
        />
      </div>
    </v-app-bar>

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
            <v-select
              v-model="select"
              :items="items"
              :rules="[(v) => !!v || 'Item is required']"
              label="Choisir un champion"
              required
            ></v-select>
            <v-select
              v-model="select"
              :items="items"
              :rules="[(v) => !!v || 'Item is required']"
              label="Choisir un champion"
              required
            ></v-select>
          </div>
          <v-btn size="x-large" append-icon="mdi-compare "> Comparer </v-btn>
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
import { shallowRef } from "vue";

const drawer = shallowRef(false);

const items = ["Home", "Matchups", "Items"];
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
