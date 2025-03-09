<template>
  <v-app-bar elevation="0" rounded="lg" class="px-4">
    <v-img
      :width="40"
      :height="40"
      src="/src/assets/logo.png"
      class="flex-0-0"
    ></v-img>
    <p class="font-weight-black text-primary">BLOG</p>
    <v-spacer></v-spacer>
    <v-btn
      color="black"
      active-color="primary"
      class="text-capitalize me-4"
      v-for="(link, index) in links"
      :key="index"
      :to="link.link"
      :text="link.name"
    />
  </v-app-bar>
</template>

<script setup>
const links = [
  { name: "Home", link: "/" },

  { name: "Contact", link: "" },
];
</script>
