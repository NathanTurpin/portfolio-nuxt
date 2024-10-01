<template>
  <div class="not-prose">
    <section v-if="pending">Loading...</section>
    <section v-else-if="error">error: {{ error }}</section>
    <section v-else>
      <ul class="grid grid-cols-1 gap-4">
        <li
          v-for="repository in computedRepos"
          :key="repository.id"
          class="border border-gray-200 dark:border-gray-800 rounded-sm p-4 hover:bg-gray-200 font-mono dark:hover:bg-gray-800"
        >
          <a :href="repository.html_url" target="_blank">
            <div class="flex items-center justify-between text-sm">
              <div class="font-semibold">
                {{ repository.name }}
              </div>
              <div>{{ repository.stargazers_count }} *</div>
            </div>
            <p>{{ repository.description }}</p>
          </a>
        </li>
      </ul>
    </section>
  </div>
</template>

<script setup>
const { error, pending, data } = await useFetch(
  "https://api.github.com/users/NathanTurpin/repos"
);

const computedRepos = computed(() => {
  return data.value
    .filter((repo) => repo.description)
    .sort((a, b) => {
      return b.stargazers_count - a.stargazers_count;
    });
});
</script>