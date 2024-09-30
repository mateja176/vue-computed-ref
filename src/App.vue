<script setup lang="ts">
import HelloWorld, { Post } from "./components/HelloWorld.vue";
import { onMounted, ref } from "vue";

const posts = ref<Post[]>([]);

onMounted(async () => {
  const postsRes = await fetch(
    "https://jsonplaceholder.typicode.com/posts?_start=0&_limit=10"
  );
  posts.value = await postsRes.json();
});

const onShuffle = () => {
  posts.value = posts.value.sort(() => (Math.random() > 0.5 ? 1 : -1));
};
</script>

<template>
  <div :style="{
    alignItems: 'center',
    display: 'flex',
    flexDirection: 'column',
    flexGrow: 1,
    gap: '1rem'
  }">
    <button @click="onShuffle">Shuffle posts</button>
    <ol>
      <li v-for="post in posts">{{ post.id }}: {{ post.title }}</li>
    </ol>
    <HelloWorld :posts="posts" />
  </div>
</template>

<style scoped></style>
