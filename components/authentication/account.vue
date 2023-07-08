<script lang="ts" setup></script>

<template>
  <div
    v-on:click="navigateTo('/account')"
    class="w-24 flex justify-center items-center cursor-pointer transition"
  >
    <Icon name="ic:outline-account-circle" size="30" class="scale-[1.25] hover:scale-[1.75]" />
  </div>
</template>

<style scoped></style>
