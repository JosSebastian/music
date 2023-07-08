<script lang="ts" setup>
const modelValue = defineModel();
const { type } = defineProps({
  type: {
    type: String,
    default: "text",
  },
});
const emit = defineEmits(["input"]);
</script>

<template>
  <input
    v-model="modelValue"
    v-bind:type="type"
    v-on:input="emit('input')"
    class="flex w-full rounded-md bg-neutral-900 border border-transparent px-3 py-3 mx-0 my-1 text-sm file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-white placeholder:text-neutral-400 disabled:cursor-not-allowed disabled:opacity-50 focus:outline-none"
  />
</template>

<style scoped></style>
