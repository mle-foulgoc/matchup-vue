<template>
  <v-app id="inspire">
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="20" md="2">
            <v-sheet min-height="268" rounded="lg"> </v-sheet>
          </v-col>

          <v-col cols="12" md="8">
            <v-sheet min-height="40vh" rounded="lg">
              <!-- <div class="d-flex justify-center align-center avatar-container" style="padding-top: 20px; gap: 40px;">
                  <v-avatar color="primary" class="responsive-avatar">32</v-avatar>
                  <v-avatar v-for="n in 4" :key="n" color="primary" class="responsive-avatar">48</v-avatar>
                </div> -->
              <Spells />
            </v-sheet>
          </v-col>

          <v-col cols="12" md="2">
            <v-sheet min-height="268" rounded="lg"> </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

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
