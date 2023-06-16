<template>
  <div class="flex items-center gap-2">
    <div class="flex items-center">
      <div
        v-for="(el, ind) in Math.floor(rating)"
        :key="ind"
        class="flex items-center"
      >
        <Icons name="star" color="#FF9017" size="20" />
      </div>

      <div v-if="rating != Math.floor(rating)" class="flex items-center">
        <Icons name="star_half" color="#FF9017" size="20" />
      </div>

      <div
        v-if="1 <= 5 - Math.ceil(rating) && rating == Math.floor(rating)"
        v-for="(el, ind) in 5 - Math.floor(rating)"
        :key="ind"
        class="flex items-center"
      >
        <Icons name="star_border" color="#FF9017" size="20" />
      </div>

      <div
        v-if="1 <= 5 - Math.ceil(rating) && rating != Math.floor(rating)"
        v-for="(el, ind) in 4 - Math.floor(rating)"
        :key="ind"
        class="flex items-center"
      >
        <Icons name="star_border" color="#FF9017" size="20" />
      </div>
    </div>

    <h6 class="body-sm pt-1">
      <span class="text-orange">{{ rating }}</span>
    </h6>
  </div>
</template>

<script setup>
defineProps(["rating"]);
</script>

<style lang="scss" scoped></style>
