<template>
  <div class="flex gap-2 my-5">
    <div class="w-10 h-10 flex items-center justify-center">
      <Icons name="account_circle" size="40" />
    </div>

    <div>
      <div class="flex gap-5 items-center mb-2">
        <h6 class="text-base">
          <span>{{ el.user.fullname }}</span>
        </h6>

        <h6 class="text-sm text-gray-500">
          <span>{{ moment(el.createdAt).fromNow() }}</span>
        </h6>
      </div>

      <h6 class="text-lg">
        <span>{{ el.body }}</span>
      </h6>
    </div>
  </div>
</template>

<script setup>
import moment from "moment";

defineProps(["el"]);
</script>

<style lang="scss" scoped></style>
