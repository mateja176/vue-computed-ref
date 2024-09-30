<script setup lang="ts">
import { computed, ref, type Ref } from "vue";

export type Post = {
  body: string;
  id: number;
  title: string;
  userId: number;
};

const props = defineProps<{ posts: Post[]; }>();

const post = computed<{ readonly posts: [], readonly id: Ref<null>; } | {
  readonly posts: [Post, ...Post[]];
  readonly id: Ref<number>;
}>(() => {
  const [firstPost, ...posts] = props.posts;
  return firstPost ? { posts: [firstPost, ...posts], id: ref(firstPost.id) } : { posts: [], id: ref(null) };
});
const selectedPost = computed<Post | null>(() => {
  const id = post.value.id.value;
  return id
    ? props.posts.find((p) => p.id === id) ?? null : null;
});
const onChange = (e: Event) => {
  if (!(e.target instanceof HTMLInputElement)) {
    return;
  }
  const id = parseInt(e.target.value);
  if (Number.isNaN(id)) {
    return;
  }
  post.value.id.value = id;
};
</script>

<template>
  <input :disabled="post.id === null" type="number" step="1" placeholder="Post id" :value="post.id.value ?? ''"
    @change="onChange" />
  <div v-if="selectedPost" :style="{
    alignSelf: 'stretch',
    display: 'flex',
    flexDirection: 'column',
    flexGrow: 1
  }">
    <h1 :title="selectedPost.title" :style="{
      margin: 0,
      overflow: 'hidden',
      textOverflow: 'ellipsis',
      whiteSpace: 'nowrap'
    }">{{
      selectedPost.title }}</h1>
  </div>
</template>

<style scoped></style>
