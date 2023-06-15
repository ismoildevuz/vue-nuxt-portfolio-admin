<template>
  <div class="w-full">
    <label
      :for="name"
      class="text-[18px] leading-[27px] font-semibold text-[#303972] mb-[16px] block w-full"
      >{{ label }} *</label
    >
    <input
      :value="value"
      @input="$emit('update:value', $event.target.value)"
      :type="type ? type : 'text'"
      :name="name"
      :id="name"
      :placeholder="placeholder"
      class="text-[14px] leading-[21px] text-[#A098AE] border border-[#C1BBEB] rounded-[5px] p-[15px] w-full focus:outline-none focus:border-[#a098ae]"
      required
    />
  </div>
</template>

<script setup>
const props = defineProps(["name", "value", "placeholder", "type", "label"]);
</script>

<style lang="scss" scoped></style>
