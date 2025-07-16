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

      <div class="d-flex flex-1-1-0 pe-3">
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
import { shallowRef } from 'vue';

const drawer = shallowRef(false);
const items = [
  { title: "Home", to: "/" },
  { title: "Matchups", to: "/matchups" },
  { title: "Items", to: "/items" },
];
</script>