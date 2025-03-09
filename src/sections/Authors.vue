<template>
  <v-responsive class="align-center mx-auto text-center mb-8 mt-10">
    <h2 class="text-grey-lighten-2 mb-8">Expolore Authors</h2>
    <v-btn
      v-for="(author, index) in authors"
      :key="index"
      :class="{ active: selectedauthor === author.name }"
      white
      rounded
      class="p-6 mx-4 mb-4 text-capitalize font-weight-bold"
      @click="selectAuthor(author.name)"
    >
      <!-- <v-avatar>
        <span class="text-h5">{{ author.initials }}</span>
      </v-avatar> -->

      {{ author.name }} ({{ author.postCount }})
    </v-btn>
    <v-btn
      class="p-6 mx-4 mb-4 text-capitalize font-weight-bold"
      @click="selectAuthor(null)"
      rounded
      >All</v-btn
    >
  </v-responsive>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
const props = defineProps({
  authors: Array,
  selectedauthor: String,
});

// DEFINE EMIT
const emit = defineEmits(["selected-author"]);

const selectAuthor = (author) => {
  emit("selected-author", author);
};
</script>
