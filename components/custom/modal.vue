<script lang="ts" setup>
const { title, tclass, description, dclass } = defineProps({
  button: String,
  bclass: String,
  title: String,
  tclass: String,
  description: String,
  dclass: String,
});
const emit = defineEmits(["close"]);
</script>

<template>
  <div class="fixed inset-0 bg-neutral-900/10 backdrop-blur-sm z-50">
    <div
      class="fixed drop-shadow-md border border-neutral-700 top-[50%] left-[50%] max-h-full h-full md:h-auto md:max-h-[85vh] w-full md:w-[90vw] md:max-w-[450px] translate-x-[-50%] translate-y-[-50%] rounded-md bg-neutral-800 p-[25px] focus:outline-none"
    >
      <icon
        v-on:click="emit('close')"
        name="ic:outline-close"
        size="20"
        class="absolute top-[20px] right-[20px] text-neutral-500 hover:text-neutral-400"
      />
      <div class="px-0 py-2">
        <p v-bind:class="tclass" class="text-2xl font-bold mb-4">
          {{ title }}
        </p>
        <p
          v-bind:class="dclass"
          class="mb-5 text-sm leading-normal"
        >
          {{ description }}
        </p>
      </div>
      <slot />
    </div>
  </div>
</template>

<style scoped></style>
