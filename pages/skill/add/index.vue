<template>
  <div class="p-10">
    <form @submit.prevent="addItem">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Skill" page="add" />

        <div class="p-[40px] pb-0">
          <div class="flex gap-[24px]">
            <div class="w-full grid grid-cols-2 h-min gap-10">
              <Input
                @update:value="data.name = $event"
                name="name"
                label="Name"
                placeholder="Enter name"
                :value="data.name"
              />
            </div>
          </div>
        </div>

        <div class="flex items-center justify-end p-10 pt-0">
          <SubmitBtn text="Add" />
        </div>
      </div>
    </form>
  </div>
</template>

<script setup>
import axios from "axios";
import { ElNotification } from "element-plus";

const data = reactive({
  name: "",
});

const addItem = (e) => {
  const newItem = {
    name: data.name,
  };

  axios
    .post(`https://nest-portfolio-xy2i.onrender.com/api/skill`, newItem)
    .then((res) => {
      ElNotification({
        title: "Added",
        type: "success",
      });

      data.name = "";

      useRouter().push({ name: "skill" });
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
</script>

<style lang="scss" scoped></style>
