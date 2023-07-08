<script setup>
const config = useRuntimeConfig();
const client = useSupabaseClient();
const user = useSupabaseUser();
const player = useState("player");
const play = (index) => {
  player.value.show = true;
  player.value.play = true;
  player.value.music = library.value;
  player.value.index = index;
};
const library = ref([]);
const getLibrary = async () => {
  const { data, error } = await client
    .from("music")
    .select("*")
    .eq("uid", user.value?.id)
    .order("id", { ascending: true });
  if (error) {
    return;
  } else {
    library.value = data;
  }
};
onMounted(async () => {
  await getLibrary();
});
</script>

<template>
  <div class="full flex flex-col">
    <div class="p-3 flex items-center justify-between text-neutral-300">
      <div class="inline-flex items-center gap-3">
        <icon name="ic:outline-library-music" size="27" />
        <span class="text-base font-medium">Library</span>
      </div>
      <MusicUpload
        v-if="user"
        v-on:upload="
          (music) => {
            library.unshift(music);
          }
        "
      />
    </div>
    <div class="max-h-[330px] m-3 p-1 flex flex-col gap-2 overflow-auto">
      <div
        v-for="(item, index) in library"
        v-bind:key="item.id"
        v-on:click="play(index)"
        class="p-1 flex items-center gap-2 rounded-md hover:bg-neutral-800 border border-transparent hover:border-neutral-700 cursor-pointer"
      >
        <img
          v-bind:src="`${config.public.supabase.url}/storage/v1/object/public/music/image/${item.image}`"
          class="w-12 h-12 rounded-md object-cover hover:scale-[1.025] transition"
        />
        <div class="w-full flex items-center justify-between">
          <div>
            <p class="text-sm font-medium">{{ item.title }}</p>
            <p class="-translate-y-0.5 text-xs font-light">{{ item.artist }}</p>
          </div>
          <MusicDownload v-bind:title="item.title" v-bind:audio="item.audio" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
