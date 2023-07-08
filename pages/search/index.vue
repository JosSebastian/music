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
  if (!search.value) {
    const { data, error } = await client.from("music").select("*");
    if (error) {
      return;
    } else {
      music.value = data;
    }
  } else {
    const { data, error } = await client
      .from("music")
      .select("*")
      .ilike("title", `%${search.value}%`)
      .order("created_at", { ascending: false });
    if (error) {
      return;
    } else {
      music.value = data;
    }
  }
}
const search = ref("");
watch(
  search,
  debounce(() => getMusic())
);
function debounce(func, timeout = 300) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      func.apply(this, args);
    }, timeout);
  };
}
onMounted(async () => {
  await getMusic();
});
</script>

<template>
  <div class="flex flex-col items-center md:items-start gap-3 m-3 overflow-auto">
    <h1 class="px-3 py-1 text-3xl">Search ~</h1>
    <CustomInput
      v-model="search"
      type="text"
      class=" my-1 border border-neutral-400"
    />
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
