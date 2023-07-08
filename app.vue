<script lang="ts" setup>
const player = useState("player", () => {
  return {
    show: false,
    play: false,
    music: [],
    index: 0,
  };
});
</script>

<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
