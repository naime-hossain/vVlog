<template>
  <v-container>
    <v-row no-gutters>
      <v-col
        v-for="(post, index) in posts"
        :key="post.id || index"
        cols="12"
        sm="12"
        md="6"
      >
        <BlogCard :post="post">
          <v-btn
            :to="`/blog/${post.title.toLowerCase().replace(/\s+/g, '-')}`"
            color="green"
            variant="tonal"
            text="Read More"
          ></v-btn>
          <UpdateBlog
            :key="post.id"
            @update-post="updateBlogPost"
            :updatePost="post"
          />

          <v-btn
            color="red"
            @click="deletePost(post.id)"
            variant="tonal"
            text="Delete"
          ></v-btn>
        </BlogCard>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { defineProps, defineEmits, watch } from "vue";

const props = defineProps({
  posts: Array,
  length: Number,
});

// DEFINE EMITS

const emit = defineEmits(["edited-post", "delete-post"]);

// EMIT POST UPDATE
const updateBlogPost = (editedpost) => {
  emit("edited-post", editedpost);
  // console.log(editedpost);
};

// EMIT POST DELETE

const deletePost = (postId) => {
  emit("delete-post", postId);
};
</script>
