<template>
  <ul>
    <li v-for="link in links" :key="link.id">
      <nuxt-link
        :to="{ path: route.path, hash: `#${link.id}` }"
        :class="{
          'ml-4': level,
          'text-green-600 dark:text-green-400': activeId === link.id,
        }"
        >{{ link.text }}</nuxt-link
      >
      <toc-links
        :links="link.children"
        :level="level + 1"
        :activeId="activeId"
      ></toc-links>
    </li>
  </ul>
</template>

<script setup>
const route = useRoute();
defineProps({
  links: Array,
  level: {
    type: Number,
    default: 0,
  },
  activeId: String,
});
</script>