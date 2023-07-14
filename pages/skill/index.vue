<template>
  <div class="min-h-screen">
    <div class="p-5">
      <AddBtn name="skill" />
    </div>

    <div class="grid gap-5 p-5">
      <div v-for="(el, ind) in data.list" :key="ind">
        <SkillCard @update-list="updateList" :el="el" />
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";

const data = reactive({
  list: [],
});

const updateList = () => {
  axios
    .get(`http://localhost:3001/api/skill`)
    .then((res) => {
      data.list = res.data;
    })
    .catch((error) => {
      const message = error?.response?.data?.message;
      if (typeof message == "object") {
        for (let i in message) {
          setTimeout(() => {
            ElNotification({
              title: "Error",
              message: message[i],
              type: "warning",
            });
          }, i * 200);
        }
      } else {
        ElNotification({
          title: "Error",
          message: message,
          type: "warning",
        });
      }
    });
};

onMounted(() => {
  updateList();
});
</script>

<style lang="scss" scoped></style>
