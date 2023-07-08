<script setup>
const config = useRuntimeConfig();
const { item } = defineProps(["item"]);
const emit = defineEmits(["play", "playpause"]);
const client = useSupabaseAuthClient();
const player = useState("player");
const show = ref(false);
watch(player.value, () => {
  if (player.value.music.at(player.value.index) == item) {
    show.value = player.value.show;
  } else {
    show.value = false;
  }
});
const user = useSupabaseUser();
const favored = ref(false);
const getFavored = async () => {
  const { data, error } = await client
    .from("favor")
    .select("*")
    .eq("music", item.id)
    .eq("uid", user.value?.id);
  if (error) {
    return;
  } else {
    if (data.length > 0) {
      favored.value = true;
    } else {
      favored.value = false;
    }
  }
};
const favor = async () => {
  if (!favored.value) {
    const { data, error } = await client.from("favor").insert({
      music: item.id,
      uid: user.value?.id,
    });
    if (error) {
      return;
    } else {
      favored.value = true;
    }
  } else {
    const { data, error } = await client
      .from("favor")
      .delete()
      .eq("music", item.id)
      .eq("uid", user.value?.id);
    if (error) {
      return;
    } else {
      favored.value = false;
    }
  }
};
onMounted(async () => {
  await getFavored();
});
</script>

<template>
  <div
    class="w-64 max-w-fit h-64 p-3 flex flex-col gap-1.5 rounded-md bg-neutral-800 hover:scale-105 transition"
  >
    <img
      v-on:click="emit('play')"
      v-on:dblclick="favor"
      v-bind:src="`${config.public.supabase.url}/storage/v1/object/public/music/image/${item.image}`"
      class="w-48 h-48 rounded-md object-cover hover:scale-[1.025] transition cursor-pointer"
    />
    <div class="flex flex-row justify-between items-center">
      <div>
        <p class="text-sm font-medium">{{ item.title }}</p>
        <p class="-translate-y-0.5 text-xs font-light">{{ item.artist }}</p>
      </div>
      <div class="flex flex-row gap-1.5">
        <CustomContainer
          v-on:click="emit('playpause')"
          v-if="show"
          class="h-full flex justify-center items-center hover:scale-105 cursor-pointer"
        >
          <icon v-if="player.play" name="ic:baseline-pause" size="21" />
          <icon v-else name="ic:baseline-play-arrow" size="21" />
        </CustomContainer>
        <CustomContainer
          v-if="favored"
          class="h-full flex justify-center items-center hover:scale-105 cursor-pointer"
        >
          <Icon name="ic:baseline-favorite" size="21" />
        </CustomContainer>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
