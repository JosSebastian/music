<script lang="ts" setup>
const open = ref(false);
const client = useSupabaseClient();
const credentials = ref({
  email: "",
  password: "",
});
const signup = async () => {
  const { data, error } = await client.auth.signUp(credentials.value);
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
      v-on:keyup.enter="signup"
      class="hover:scale-[1.125] transition"
    >
      <p>Sign Up</p>
    </CustomButton>
    <CustomModal
      v-if="open"
      v-on:keyup.esc="open = false"
      v-on:close="open = false"
      title="Sign Up ~"
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
          v-on:click="signup"
          class="w-full my-2 border hover:scale-[1.025]"
        >
          <p>Sign Up</p>
        </CustomButton>
      </div>
    </CustomModal>
  </div>
</template>

<style scoped></style>
