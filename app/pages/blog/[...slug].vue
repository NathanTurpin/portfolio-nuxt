<template>
  <article
    class="max-w-none prose dark:prose-invert prose-pre:bg-white dark:prose-pre:bg-gray-800 prose-pre:text-gray-700 dark:prose-pre:text-gray-300"
  >
    <ContentDoc>
      <template #not-found>
        <h1>Document not found (404)</h1>
      </template>
      <template v-slot="{ doc }">
        <div class="grid grid-cols-6 gap-16">
          <div
            :class="{
              'col-span-6 mdcol-span-4': doc.toc,
              'col-span-6': !doc.toc,
            }"
          >
            <ContentRenderer :value="doc" />
          </div>
          <div class="hidden md:col-span-2 md:block not-prose" v-if="doc.toc">
            <aside class="sticky top-8">
              <div class="font-semibold mb-2">Table of Contents</div>
              <nav>
                <toc-links :links="doc.body.toc.links" :activeId="activeId" />
              </nav>
            </aside>
          </div>
        </div>
      </template>
    </ContentDoc>
  </article>
</template>

<script setup>
const activeId = ref(null);

onMounted(() => {
  const callback = (entries) => {
    console.log(entries);
    for (const entry of entries) {
      if (entry.isIntersecting) {
        activeId.value = entry.target.id;
        break;
      }
    }
  };

  const observer = new IntersectionObserver(callback, {
    root: null,
    threshold: 0.5,
  });

  const elements = document.querySelectorAll("h2,h3");

  for (const element of elements) {
    observer.observe(element);
  }
  onBeforeMount(() => {
    console.log("onBeforeUnmount");

    for (const element of elements) {
      observer.unobserve(element);
    }
  });
});
</script>