<template>
  <v-responsive
    class="align-center mx-auto text-center pt-12 pb-4"
    max-width="900"
  >
    <h1 class="font-weight-bold text-h2 mb-6" v-html="title" />
    <p class="text-body-1 mx-auto w-66 text-grey-darken-1" v-html="subtitle" />
  </v-responsive>
</template>

<script setup>
//
const title =
  'Vue & <span class="text-primary">Vuetify:</span> Stories of Innovation and Challenges';
const subtitle =
  "Exploring the art of UI development, overcoming coding hurdles, and mastering the Vuetify ecosystem.";
</script>
