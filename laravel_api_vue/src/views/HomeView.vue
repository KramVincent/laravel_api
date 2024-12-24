<script setup>
import { usePostsStore } from "@/stores/posts";
import { onMounted, ref } from "vue";
import { RouterLink } from "vue-router";

const postsStore = usePostsStore();
const posts = ref([]);

onMounted(async () => {
  try {
    posts.value = await postsStore.getAllPosts();
    console.log(posts.value); // Debugging: Ensure posts data is included
  } catch (error) {
    console.error('Error fetching posts:', error);
  }
});
</script>

<template>
  <main>
    <h1 class="title">Latest Posts</h1>

    <div v-if="posts.length > 0">
      <div
        v-for="post in posts"
        :key="post.id"
        class="border-l-4 border-blue-500 pl-4 mb-12"
      >
        <h2 class="font-bold text-3xl">{{ post.title }}</h2>
        <p class="text-xs text-slate-600 mb-4">
          Posted by {{ post.user?.name || 'Unknown Author' }}
        </p>
        <p>
          {{ post.body }}
          <RouterLink
            :to="{ name: 'show', params: { id: post.id } }"
            class="text-blue-500 font-bold underline"
          >Read more...</RouterLink>
        </p>
      </div>
    </div>
    <div v-else>
      <h2 class="title">There are no posts</h2>
    </div>
  </main>
</template>