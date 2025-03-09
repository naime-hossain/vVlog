<template>
  <div class="pa-4 text-center">
    <v-dialog v-model="dialog" max-width="600">
      <template v-slot:activator="{ props: activatorProps }">
        <v-btn
          class="text-none font-weight-regular"
          prepend-icon="mdi-pen"
          text="Update Blog"
          variant="tonal"
          v-bind="activatorProps"
          color="primary"
        ></v-btn>
      </template>

      <v-card prepend-icon="mdi-account" title="Update Blog">
        <v-card-text>
          <v-form @submit.prevent>
            <v-row dense>
              <v-col cols="12" md="6" sm="6">
                <v-text-field
                  v-model="updatePost.author"
                  :rules="[rules.required, rules.maxLength(20)]"
                  label="Author Name*"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" md="12" sm="12">
                <v-text-field
                  label="Title"
                  v-model="updatePost.title"
                  :rules="[rules.required, rules.maxLength(50)]"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" md="12" sm="12">
                <v-textarea
                  :rules="[rules.required, rules.maxLength(200)]"
                  v-model="updatePost.text"
                  label="Update Blog Content"
                  rows="4"
                  variant="filled"
                  cols="12"
                  required
                ></v-textarea>
              </v-col>
              <v-btn
                variant="tonal"
                color="primary"
                class="mt-2"
                type="submit"
                @click="submit()"
                block
                :disabled="!isFormValid"
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
import { ref, defineEmits, computed, defineProps } from "vue";

const dialog = ref(false);

// DEFINE PROPS
const props = defineProps({
  updatePost: {
    type: Object,
    required: true,
  },
});

const updatedPost = ref(props.updatePost);

// DEFINE VALIDATION RULES
const rules = {
  required: (v) => !!v || "This field is required",
  maxLength: (max) => (v) => {
    const lengthCount = v.trim().length;
    return lengthCount <= max || ` should not exceed ${max} Lengths.`;
  },
};

// DEFINE EMITS
const emit = defineEmits(["update-post"]);

// CHECK IF THE FORM IS VALIDATED
const isFormValid = computed(() => {
  return (
    props.updatePost.author.trim() &&
    props.updatePost.title.trim() &&
    props.updatePost.text.trim() &&
    props.updatePost.author.length <= 20 &&
    props.updatePost.title.length <= 50 &&
    props.updatePost.text.length <= 200
  );
});

const submit = () => {
  //FORM VALIDATION
  if (!isFormValid.value) return;

  // EMIT UPDATED POST TO BLOGSECTION
  emit("update-post", updatedPost.value);

  // DIALOG CLOSE
  dialog.value = false;
};
</script>
