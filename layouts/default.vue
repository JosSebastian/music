<script setup>
const player = useState("player");
</script>

<template>
  <div>
    <div class="flex bg-black">
      <BarSide />
      <CustomContainer
        v-bind:class="player.show ? 'h-[calc(100vh-96px)]' : ''"
        class="mx-0 my-2"
      >
        <BarTop />
        <slot />
        <MusicPlayer />
      </CustomContainer>
    </div>
  </div>
</template>

<style scoped></style>
