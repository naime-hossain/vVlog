<template>
  <v-card class="mx-auto my-2 me-4 rounded-lg pa-4" elevation="4">
    <v-card-item>
      <v-card-title>{{ title }}</v-card-title>
      <v-card-subtitle> By: {{ author }} || Date: {{ date }} </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      {{ text }}
    </v-card-text>

    <v-card-actions>
      <slot></slot>
    </v-card-actions>
  </v-card>
</template>

<script setup>
import { defineProps } from "vue";

// DEFINE PROPS
const props = defineProps({
  post: {
    type: Object,
    required: true,
  },
});

// DESTRUCTURE THE PROPS
const { title, author, date, text } = props.post;
</script>
