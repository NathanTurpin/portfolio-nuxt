<template>
  <div class="flex space-x-2 items-center">
    <div class="text-gray-500 text-xs" v-if="showNextModeLabel">
      Change to {{ nextMode }}
    </div>
    <button
      @click="toogleMode"
      @mouseenter="showNextModeLabel = true"
      @mouseleave="showNextModeLabel = false"
      class="hover:bg-gray-100 dark:hover:bg-gray-600 px-2 py-1 text-gray-500 text-4xl md:text-base"
    >
      {{ nextModeIcon }}
    </button>
  </div>
</template>


<script setup>
const showNextModeLabel = ref(false);
const colorMode = useColorMode();
const modes = ["system", "light", "dark"];
const nextModeIcons = {
  system: "🌓",
  light: "🌕",
  dark: "🌑",
};

const nextMode = computed(() => {
  const currentModesIndex = modes.indexOf(colorMode.preference);

  let nextModeIndex = null;

  if (currentModesIndex + 1 === modes.length) {
    nextModeIndex = 0;
  } else {
    nextModeIndex = currentModesIndex + 1;
  }

  return modes[nextModeIndex];
});

const nextModeIcon = computed(() => {
  return nextModeIcons[nextMode.value];
});

const toogleMode = () => {
  colorMode.preference = nextMode.value;
};
</script>