<script lang="ts" setup>
const open = ref(false);
const { title, audio } = defineProps(["title", "audio"]);
const client = useSupabaseClient();
const download = async () => {
  const { data } = client.storage.from("music/audio").getPublicUrl(audio);
  if (!data) {
  } else {
    window.open(data.publicUrl, "_blank");
  }
};
</script>

<template>
  <div>
    <icon
      v-on:click="open = true"
      name="ic:baseline-minus"
      size="21"
      class="my-0 mx-3 cursor-pointer text-neutral-300 hover:text-white transition"
    />
    <CustomModal
      v-if="open"
      v-on:keyup.esc="open = false"
      v-on:keyup.enter="download"
      v-on:close="open = false"
      title="Download Music ~"
    >
      <CustomButton
        v-on:click="download"
        class="w-full my-2 border hover:scale-[1.025]"
      >
        <p>Download</p>
      </CustomButton>
    </CustomModal>
  </div>
</template>

<style scoped></style>
