<template>
  <div class="home">
    <h1>Home</h1>
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length">
      <PostList :posts="posts" />
    </div>
    <div v-else>Loading...</div>

    <div class="copytext-box">
      <p>{{ copyme }}</p>
      <input ref="copyinput" type="text" class="copyinput" v-model="copyme" />
      <button class="copy-btn" @click="handleCopy">
        {{ copied ? "Copied" : "Copy" }}
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import PostList from "../components/PostList.vue";
import getPosts from "../composables/getPosts.js";

export default {
  name: "Home",
  components: {
    PostList,
  },
  setup() {
    const { posts, error, load } = getPosts();
    load();
    const copied = ref(false);
    const copyme = ref();
    const copyinput = ref(copyme);
    const handleCopy = () => {
      navigator.clipboard.writeText(copyinput.value);
      // alert("Copied: " + copyinput.value);
      copied.value = true;
      setTimeout(() => {
        copied.value = false;
      }, 2000);
    };

    return { posts, error, copyme, copied, handleCopy };
  },
};
</script>
