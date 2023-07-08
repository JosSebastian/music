<script lang="ts" setup>
const config = useRuntimeConfig();
const player = useState<{
  show: boolean;
  play: boolean;
  music: any[];
  index: number;
}>("player");
const play = () => {
  var audio = document.getElementById("audio") as HTMLAudioElement;
  if (player.value.play) {
    audio.pause();
    player.value.play = false;
  } else {
    audio.play();
    player.value.play = true;
  }
};
const next = () => {
  if (player.value.index + 1 >= player.value.music.length) {
    player.value.index = 0;
  } else {
    player.value.index = player.value.index + 1;
  }
};
const prev = () => {
  if (player.value.index - 1 < 0) {
    player.value.index = player.value.music.length - 1;
  } else {
    player.value.index = player.value.index - 1;
  }
};
</script>

<template>
  <div
    v-if="player.show"
    class="fixed bottom-0 left-0 z-50 w-full grid grid-cols-3 items-center justify-between px-3 py-2 rounded-t-md bg-neutral-900"
  >
    <div class="flex gap-3">
      <img
        v-bind:src="`${
          config.public.supabase.url
        }/storage/v1/object/public/music/image/${
          player.music.at(player.index).image
        }`"
        class="w-16 h-16 rounded-md object-cover"
      />
      <div class="w-full flex flex-col justify-center">
        <p class="text-xl font-medium">
          {{ player.music.at(player.index).title }}
        </p>
        <p class="-translate-y-0.5 text-sm font-light">
          {{ player.music.at(player.index).artist }}
        </p>
      </div>
    </div>
    <div class="flex gap-3 justify-center">
      <button class="p-2 bg-white rounded-full hover:scale-105 transition">
        <icon
          v-on:click="prev"
          v-bind:name="`ic:baseline-skip-previous`"
          size="30"
          class="cursor-pointer text-neutral-700 hover:text-neutral-900 transition"
        />
      </button>

      <button class="p-2 bg-white rounded-full hover:scale-105 transition">
        <icon
          v-on:click="play"
          v-bind:name="
            player.play ? 'ic:baseline-pause' : 'ic:baseline-play-arrow'
          "
          size="30"
          class="cursor-pointer text-neutral-700 hover:text-neutral-900 transition"
        />
      </button>
      <button class="p-2 bg-white rounded-full hover:scale-105 transition">
        <icon
          v-on:click="next"
          v-bind:name="`ic:baseline-skip-next`"
          size="30"
          class="cursor-pointer text-neutral-700 hover:text-neutral-900 transition"
        />
      </button>
    </div>
    <div>
      <audio
        v-on:ended="next"
        autoplay
        v-bind:src="`${
          config.public.supabase.url
        }/storage/v1/object/public/music/audio/${
          player.music.at(player.index).audio
        }`"
        id="audio"
      ></audio>
    </div>
  </div>
</template>

<style scoped></style>
