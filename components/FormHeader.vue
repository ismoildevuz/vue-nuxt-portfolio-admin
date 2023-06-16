<template>
  <div class="bg-[#4D44B5] px-[40px] py-[12px] rounded-t-[20px]">
    <div>
      <h3 class="text-[24px] leading-[36px] font-bold text-white">
        {{ page == "add" ? "Add new" : page == "edit" ? "Edit your" : "" }}
        {{ name }}
      </h3>
    </div>
  </div>
</template>

<script setup>
defineProps(["name", "page"]);
</script>

<style lang="scss" scoped></style>
