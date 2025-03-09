<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-card v-if="post" class="pa-4">
          <v-card-title class="text-h5 font-weight-bold">
            {{ post.title }}
          </v-card-title>

          <v-card-subtitle class="text-body-2 text-grey-darken-1">
            <strong>Author:</strong> {{ post.author }} | <strong>Date:</strong>
            {{ post.date }}
          </v-card-subtitle>

          <v-card-text class="mt-3 text-body-1">
            {{ post.text }}
          </v-card-text>

          <v-divider class="my-4"></v-divider>

          <v-card-actions>
            <v-btn to="/" variant="tonal" color="primary">
              <v-icon start>mdi-arrow-left</v-icon>
              Back to Blogs
            </v-btn>
          </v-card-actions>
        </v-card>

        <v-alert v-else type="error" prominent class="mt-4">
          <v-alert-title>Post Not Found</v-alert-title>
          <p>The post with title "{{ route.params.title }}" does not exist.</p>
          <v-btn to="/" color="primary" variant="tonal">
            <v-icon start>mdi-arrow-left</v-icon>
            Go Back
          </v-btn>
        </v-alert>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();

const posts = ref([]);
onMounted(() => {
  let localStoragePosts = JSON.parse(localStorage.getItem("Posts"));

  posts.value = localStoragePosts;
});
// Get the blog post by ID or Title
const post = computed(() => {
  return posts.value.find(
    (p) =>
      p.id === route.params.title ||
      p.title.toLowerCase().replace(/\s+/g, "-") === route.params.title
  );
});
</script>
