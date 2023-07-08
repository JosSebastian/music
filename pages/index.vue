<script setup>
const client = useSupabaseAuthClient();
const player = useState("player");
const play = (index) => {
  player.value.show = true;
  player.value.play = true;
  player.value.music = music.value;
  player.value.index = index;
};
const playpause = () => {
  player.value.play = !player.value.play;
};
const music = ref([]);
async function getMusic() {
  const { data, error } = await client.from("music").select("*");
  if (error) {
    return;
  } else {
    music.value = data;
  }
}
onMounted(async () => {
  await getMusic();
});
</script>

<template>
  <div class="flex flex-col items-center md:items-start gap-3 m-3">
    <h1 class="px-3 py-1 text-3xl">Music ~</h1>
    <div
      class="h-[548px] p-3 flex flex-wrap gap-3 justify-center lg:justify-normal overflow-auto"
    >
      <MusicItem
        v-for="(item, index) in music"
        v-bind:key="item.id"
        v-bind:item="item"
        v-on:play="play(index)"
        v-on:playpause="playpause"
      />
    </div>
  </div>
</template>

<style scoped></style>
