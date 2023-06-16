<template>
  <button
    type="submit"
    class="group addButton py-3 px-5 flex items-center justify-center bg-[#4D44B5] rounded-full hover:bg-white border-2 border-[#4D44B5] cursor-pointer select-none"
  >
    <h5
      class="text-[18px] leading-[27px] text-white group-hover:text-[#4D44B5]"
    >
      {{ text }}
    </h5>
  </button>
</template>

<script setup>
defineProps(["text"]);
</script>

<style lang="scss" scoped></style>
