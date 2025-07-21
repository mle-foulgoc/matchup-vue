<template>
  <v-layout>
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
          :key="item.title"
          class="text-none"
          :to="item.to"
          :text="item.title"
          variant="text"
        />
      </div>

      <div class="d-flex flex-1-1-0 pe-3 align-center" style="gap: 12px">
        <!-- <v-btn
          icon
          variant="text"
          @click="toggleTheme"
          :aria-label="isDark ? 'Activer le thème clair' : 'Activer le thème sombre'"
        >
          <v-icon>{{ isDark ? 'mdi-weather-sunny' : 'mdi-weather-night' }}</v-icon>
        </v-btn> -->
        <v-btn
          append-icon="mdi-chevron-right"
          class="ms-auto text-none"
          slim
          text="Login"
        />
      </div>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-layout>
</template>

<script lang="ts" setup>
import { shallowRef } from "vue";
import { useTheme } from "vuetify";

const drawer = shallowRef(false);
const items = [
  { title: "Home", to: "/" },
  { title: "Matchups", to: "/matchups" },
  { title: "Items", to: "/items" },
];

const theme = useTheme();
const isDark = computed(() => theme.global.current.value.dark);
function toggleTheme() {
  theme.global.name.value = isDark.value ? "light" : "dark";
}
</script>
