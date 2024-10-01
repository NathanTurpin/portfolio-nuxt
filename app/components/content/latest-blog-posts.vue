<template>
  <blog-posts v-slot="{ posts }" :limit="limit">
    <div v-for="post in posts" :key="post.title">
      <nuxt-link :to="post._path">{{ post.title }}</nuxt-link>
    </div>
  </blog-posts>
</template>

<script setup>
defineProps({
  limit: {
    type: Number,
    default: null,
  },
});
</script>