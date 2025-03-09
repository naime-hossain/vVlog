<template>
  <div class="pa-4 text-center">
    <v-dialog v-model="dialog" max-width="600">
      <template v-slot:activator="{ props: activatorProps }">
        <v-btn
          class="text-none font-weight-regular mb-4"
          prepend-icon="mdi-pen"
          text="Create Blog"
          variant="tonal"
          v-bind="activatorProps"
          color="primary"
        ></v-btn>
      </template>

      <v-card prepend-icon="mdi-account" title="Add New Blog">
        <v-card-text>
          <v-form @submit.prevent>
            <v-row dense>
              <!-- AUTHOR FIELD -->
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="author"
                  :rules="[rules.required, rules.maxWords(10)]"
                  label="Author Name*"
                  required
                ></v-text-field>
              </v-col>

              <!-- TITLE FIELD-->
              <v-col cols="12" md="12">
                <v-text-field
                  v-model="title"
                  :rules="[rules.required, rules.maxWords(20)]"
                  label="Title"
                  required
                ></v-text-field>
              </v-col>

              <!-- BLOG TEXT FILED -->
              <v-col cols="12" md="12">
                <v-textarea
                  v-model="text"
                  :rules="[rules.required, rules.maxWords(200)]"
                  label="Add Blog Content"
                  rows="4"
                  variant="filled"
                  required
                ></v-textarea>
              </v-col>

              <!-- SUBMIT BUTTON -->
              <v-btn
                variant="tonal"
                color="primary"
                class="mt-2"
                :disabled="!isFormValid"
                @click="submit"
                block
              >
                Save Post
              </v-btn>
            </v-row>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import { ref, defineEmits, computed } from "vue";

// State variables for form data
const dialog = ref(false);
const author = ref("");
const text = ref("");
const title = ref("");

// Validation rules
const rules = {
  required: (v) => !!v || "This field is required",

  maxWords: (max) => (v) => {
    const wordCount = v.trim().length;
    return wordCount <= max || ` should not exceed ${max} words.`;
  },
};

// CHECK IF THE FORM IS VALIDATED
const isFormValid = computed(() => {
  return (
    author.value.trim() &&
    title.value.trim() &&
    text.value.trim() &&
    author.value.length <= 20 &&
    title.value.length <= 20 &&
    text.value.length <= 200
  );
});

// DEFINE EMIT
const emit = defineEmits(["add-post"]);

// SUBMIT FORM
const submit = () => {
  if (!isFormValid.value) return;

  // NEW POST ARRAY
  const newPost = {
    id: crypto.randomUUID(),
    author: author.value,
    title: title.value,
    text: text.value,
    date: new Date().toLocaleString(),
  };

  //EMIT THE ADD-POST WITH NEW POST
  emit("add-post", newPost);

  // CLOSE DIALOG
  dialog.value = false;
  resetForm();
};

// RESET FORM VALUES
const resetForm = () => {
  author.value = "";
  title.value = "";
  text.value = "";
};
</script>
