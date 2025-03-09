<template>
  <v-container d-flex flex-col>
    <HeroSection />
    <AddBlog @add-post="addBlogPost" />
    <v-divider></v-divider>

    <Authors
      :authors="authorCount"
      :selectedauthor="selectedAuthor"
      @selected-author="filterAuthor"
    />

    <BlogSection
      :key="updateCounter"
      :posts="filteredBlogPosts"
      @edited-post="updateBlogPost"
      @delete-post="deleteBlogPost"
    />
  </v-container>
</template>

<script setup>
import Authors from "@/sections/Authors.vue";
import BlogSection from "@/sections/BlogSection.vue";

import { posts } from "@/data/posts";
import { onMounted, computed, ref } from "vue";

let blogPosts = ref([]);
const authorCount = ref([]);
const selectedAuthor = ref();
const updateCounter = ref(0);

// SETTING THE POSTS TO LOCALSTORAGE
onMounted(() => {
  let localStoragePosts = JSON.parse(localStorage.getItem("Posts"));

  if (!localStoragePosts) {
    localStorage.setItem("Posts", JSON.stringify(posts));
    localStoragePosts = JSON.parse(localStorage.getItem("Posts"));
    blogPosts.value = localStoragePosts;
  } else {
    blogPosts.value = localStoragePosts;
  }
  // COUNT AUTHOR POSTS
  authorCounts();
});

// FILTER AUTHOR ON CLICK AUTHOR BUTTON
const filterAuthor = (author) => {
  selectedAuthor.value = author;
  console.log(selectedAuthor.value);
};

// FILTER BLOG POSTS BY SELECTED AUTHOR

const filteredBlogPosts = computed(() => {
  return selectedAuthor.value
    ? blogPosts.value.filter((post) => post.author === selectedAuthor.value)
    : blogPosts.value; // SHOWS ALL POST INITIALLY
});

// watch(filteredBlogPosts, (newPosts) => {
//   console.log("Filtered blog posts updated:", newPosts);
// });

// Add new Blog Post

const addBlogPost = (post) => {
  // SAVE THE POST TO BLOGPOSTS ARRAY
  blogPosts.value.unshift(post);

  //UPDATE THE LOCALSTORAGE AFTER SAVE
  updateLocalStorage();
  // Update Author count
  authorCounts();
  //INCREASE COUNTER TO MAKE BLOGSECTION REACTIVE UPON CHANGE IF NOT
  updateCounter.value++;
};

// UPDATE BLOGPOST
const updateBlogPost = (updatedPost) => {
  // GETTING POST INDEX OF UPDATED POST FROM BLOGPOSTS ARRAY
  const postIndex = blogPosts.value.findIndex(
    (post) => post.id === updatedPost.id
  );

  if (postIndex !== -1) {
    // UPDATING THE POST
    blogPosts.value[postIndex] = { ...updatedPost };

    // UPDATING LOCALSTORAGE AND AUTHOR COUNT

    updateLocalStorage();
    authorCounts();
    updateCounter.value++;
  } else {
    console.log("Post not found.");
  }
};

// DELETE A SPECIFIC POST
const deleteBlogPost = (postId) => {
  const isConfirmed = window.confirm(
    "Are you sure you want to delete this post?"
  );
  if (isConfirmed) {
    // FIND AND FILTER OUT SPECIFIC POST BY ID
    blogPosts.value = blogPosts.value.filter((post) => post.id !== postId);
    // UPDATE LOCALSTORAGE AND AUTHOR COUNT

    updateLocalStorage();
    authorCounts();
    //INCREASE COUNTER
    updateCounter.value++;
    alert("Post deleted successfully!");
  } else {
    alert("Post deletion canceled.");
  }
};

// UPDATE LOCALSTORAGE WITH UPDATED BLOGPOSTS
const updateLocalStorage = () => {
  localStorage.setItem("Posts", JSON.stringify(blogPosts.value));
};

// COUNT NUMBER OF AUTHOR AND THIER POSTS
const authorCounts = () => {
  // RETURN EMPTY OBJECT IF THERE IS NO POSTS
  if (!blogPosts.value || blogPosts.value.length === 0) {
    return {};
  }
  // COUNTING AUTHOR AND POSTS
  authorCount.value = blogPosts.value.reduce((acc, post) => {
    const existingAuthor = acc[post.author];

    if (existingAuthor) {
      existingAuthor.postCount += 1;
    } else {
      acc[post.author] = { name: post.author, postCount: 1 };
    }

    return acc;
  }, {});

  // CONVERT THE OBJECT TO ARRAY
  authorCount.value = Object.values(authorCount.value);
};
</script>
