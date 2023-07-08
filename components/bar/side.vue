<script lang="ts" setup>
type Routes = Route[];
type Route = {
  icon: string;
  text: string;
  path: string;
  active: boolean;
};
const router = useRouter();
const navigation = computed(() => {
  const routes: Routes = [
    {
      icon: "ic:outline-home",
      text: "Home",
      path: "/",
      active: router.currentRoute.value.path === "/",
    },
    {
      icon: "ic:outline-search",
      text: "Search",
      path: "/search",
      active: router.currentRoute.value.path === "/search",
    },
  ];
  return routes;
});
</script>

<template>
  <div class="h-screen flex">
    <div class="w-[350px] h-full p-2 hidden md:flex flex-col gap-2 bg-black">
      <CustomContainer class="flex-grow-0">
        <div class="p-1 flex flex-col gap-4">
          <CustomItem
            v-for="(route, index) in navigation"
            v-bind:key="index"
            v-bind:item="route"
            v-on:click="navigateTo(route.path)"
            v-bind:class="{ 'text-white': route.active }"
          />
        </div>
      </CustomContainer>
      <CustomContainer>
        <MusicLibrary />
      </CustomContainer>
    </div>
  </div>
</template>

<style scoped></style>
