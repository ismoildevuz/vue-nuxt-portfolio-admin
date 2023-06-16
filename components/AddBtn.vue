<template>
  <RouterLink
    :to="`/${name}/add`"
    class="w-[210px] h-[60px] flex items-center justify-center bg-[#4D44B5] rounded-full hover:bg-[#443C9E] hover:cursor-pointer select-none"
  >
    <Icons name="add" color="white" />
  </RouterLink>
</template>

<script setup>
defineProps(["name"]);
</script>

<style lang="scss" scoped></style>
