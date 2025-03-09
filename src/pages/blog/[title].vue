<template>
  <div v-if="post">
    <h1>{{ post.title }}</h1>
    <p><strong>Author:</strong> {{ post.author }} || Date : {{ post.date }}</p>
    <p>{{ post.text }}</p>
    <router-link to="/">Back to Blogs</router-link>
  </div>
  <div v-else>
    <p>Post not found.</p>
    {{ route.params.title }}
    <router-link to="/">Go back</router-link>
  </div>
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
