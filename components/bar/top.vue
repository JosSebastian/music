<script lang="ts" setup>
const router = useRouter();
const user = useSupabaseUser();
</script>

<template>
  <div class="h-fit p-7">
    <div class="w-full flex items-center justify-between">
      <div class="hidden md:flex gap-2">
        <CustomNavigation
          v-on:click="router.back()"
          class="opacity-25 hover:opacity-75"
        >
          <icon name="ic:baseline-chevron-left" size="30" />
        </CustomNavigation>
        <CustomNavigation
          v-on:click="router.forward()"
          class="opacity-25 hover:opacity-75"
        >
          <icon name="ic:baseline-chevron-right" size="30" />
        </CustomNavigation>
      </div>
      <div class="flex md:hidden gap-2">
        <CustomNavigation
          v-on:click="navigateTo('/')"
          class="p-2 opacity-80 hover:opacity-50 transition"
        >
          <icon name="ic:outline-home" size="30" />
        </CustomNavigation>
        <CustomNavigation
          v-on:click="navigateTo('/search')"
          class="p-2 opacity-80 hover:opacity-50 transition"
        >
          <icon name="ic:outline-search" size="30" />
        </CustomNavigation>
      </div>
      <div v-if="!user" class="flex justify-center items-center gap-2">
        <AuthenticationSignUp />
        <AuthenticationSignIn />
      </div>
      <div v-else class="flex justify-center items-center gap-2">
        <AuthenticationSignOut />
        <AuthenticationAccount />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
