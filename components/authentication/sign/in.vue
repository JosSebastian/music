<script lang="ts" setup>
const open = ref(false);
const client = useSupabaseClient();
const credentials = ref({
  email: "",
  password: "",
});
const signin = async () => {
  const { data, error } = await client.auth.signInWithPassword(
    credentials.value
  );
  if (error) {
  } else {
    open.value = false;
  }
};
</script>

<template>
  <div>
    <CustomButton
      v-on:click="open = true"
      v-on:keyup.esc="open = false"
      class="bg-white text-black hover:scale-[1.125] transition"
    >
      <p>Sign In</p>
    </CustomButton>
    <CustomModal
      v-if="open"
      v-on:keyup.esc="open = false"
      v-on:keyup.enter="signin"
      v-on:close="open = false"
      title="Sign In ~"
    >
      <div class="flex flex-col gap-3">
        <div>
          <label class="m-1">Email:</label>
          <CustomInput v-model="credentials.email" type="email" />
        </div>
        <div>
          <label class="m-1">Password:</label>
          <CustomInput v-model="credentials.password" type="password" />
        </div>
        <CustomButton
          v-on:click="signin"
          class="w-full my-2 border hover:scale-[1.025]"
        >
          <p>Sign In</p>
        </CustomButton>
      </div>
    </CustomModal>
  </div>
</template>

<style scoped></style>
