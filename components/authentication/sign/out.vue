<script lang="ts" setup>
const client = useSupabaseClient();
const signout = async () => {
  const { error } = await client.auth.signOut();
  if (error) {
    console.error(error);
  }
};
</script>

<template>
  <div>
    <CustomButton
      v-on:click="signout"
      class="bg-white text-black hover:scale-[1.125] transition"
    >
      <p>Sign Out</p>
    </CustomButton>
  </div>
</template>

<style scoped></style>
