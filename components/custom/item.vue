<script lang="ts" setup>
type Item = {
  icon: string;
  text: string;
};
const { item } = defineProps({
  item: {
    type: Object as PropType<Item>,
    required: true,
    default: {
      icon: "",
      text: "",
    },
  },
});
</script>

<template>
  <div
    class="w-full h-auto p-1 flex flex-row items-center gap-3 text-base font-medium text-neutral-300 hover:text-white transition cursor-pointer"
  >
    <icon v-if="item.icon" v-bind:name="item.icon" size="30" />
    <span v-if="item.text">{{ item.text }}</span>
  </div>
</template>

<style scoped></style>
