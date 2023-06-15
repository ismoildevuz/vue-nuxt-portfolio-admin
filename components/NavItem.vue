<template>
  <nuxt-link
    :to="link"
    :id="id"
    class="flex items-center pl-[20px] py-[12px] gap-[24px] rounded-l-full"
  >
    <Icons
      :name="icon"
      :color="useRoute().fullPath == link ? '#4d44b5' : '#C1BBEB'"
    />

    <h5 class="text-[18px] leading-[27px] font-medium text-[#C1BBEB]">
      {{ title }}
    </h5>
  </nuxt-link>
</template>

<script setup>
const props = defineProps(["link", "title", "icon", "id"]);
</script>

<style lang="scss" scoped>
.router-link-exact-active {
  background-color: white;
  h5 {
    color: #4d44b5;
  }
}
</style>
