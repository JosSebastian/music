<script setup>
const emit = defineEmits(["upload"]);
const open = ref(false);
const client = useSupabaseClient();
const user = useSupabaseUser();
const music = ref({
  title: "",
  artist: "",
});
const afile = ref(null);
const uafile = (input) => {
  afile.value = input.target.files[0];
};
const ifile = ref(null);
const uifile = (input) => {
  ifile.value = input.target.files[0];
};
const upload = async () => {
  if (music.value.title == "" || music.value.artist == "") {
    console.log("Please fill out all fields.");
    return;
  }

  const { data: adata, error: aerror } = await client.storage
    .from("music/audio")
    .upload(`${music.value.title}-${music.value.artist}`, afile.value, {
      cacheControl: "3600",
      upsert: false,
    });
  if (aerror) {
    console.log(aerror);
    return;
  }

  const { data: idata, error: ierror } = await client.storage
    .from("music/image")
    .upload(`${music.value.title}-${music.value.artist}`, ifile.value, {
      cacheControl: "3600",
      upsert: false,
    });
  if (ierror) {
    console.log(ierror);
    return;
  }

  const { error } = await client.from("music").insert({
    uid: user.value.id,
    title: music.value.title,
    audio: adata.path,
    image: idata.path,
    artist: music.value.artist,
  });

  if (error) {
    console.log(error);
    return;
  } else {
    emit("upload", {
      uid: user.value.id,
      title: music.value.title,
      audio: adata.path,
      image: idata.path,
      artist: music.value.artist,
    });
    open.value = false;
    return;
  }
};
</script>

<template>
  <div>
    <icon
      v-on:click="open = true"
      name="ic:baseline-plus"
      size="21"
      class="cursor-pointer hover:text-white transition"
    />
    <CustomModal
      v-if="open"
      v-on:keyup.esc="open = false"
      v-on:keyup.enter="upload"
      v-on:close="open = false"
      title="Upload Music ~"
    >
      <div class="flex flex-col gap-3">
        <div>
          <label class="m-1">Title:</label>
          <CustomInput v-model="music.title" type="text" />
        </div>
        <div>
          <label class="m-1">Artist:</label>
          <CustomInput v-model="music.artist" type="text" />
        </div>
        <div>
          <label class="m-1">Music:</label>
          <CustomInput v-on:change="uafile" type="file" />
        </div>
        <div>
          <label class="m-1">Art:</label>
          <CustomInput v-on:change="uifile" type="file" />
        </div>
        <CustomButton
          v-on:click="upload"
          class="w-full my-2 border hover:scale-[1.025]"
        >
          <p>Upload</p>
        </CustomButton>
      </div>
    </CustomModal>
  </div>
</template>

<style scoped></style>
