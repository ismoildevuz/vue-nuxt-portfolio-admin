<template>
  <div v-if="data.loaded && data.item" class="p-10">
    <form @submit.prevent="updateItem">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Skill" page="edit" />

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
          <SubmitBtn text="Save" />
        </div>
      </div>
    </form>
  </div>

  <div v-else-if="data.loaded">
    <Error name="Skill" path="skill" />
  </div>
</template>

<script setup>
import axios from "axios";
import { ElNotification } from "element-plus";

const { id } = useRoute().params;

const data = reactive({
  name: "",
  item: null,
  loaded: false,
});

const updateItem = (e) => {
  const item = {
    name: data.name,
  };

  axios
    .patch(`https://nest-portfolio-xy2i.onrender.com/api/skill/${id}`, item)
    .then((res) => {
      ElNotification({
        title: "Updated",
        type: "success",
      });

      data.name = "";

      data.item = null;
      data.loaded = false;

      useRouter().go(-1);
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
  axios
    .get(`https://nest-portfolio-xy2i.onrender.com/api/skill/${id}`)
    .then((res) => {
      data.item = res.data;
      data.loaded = true;

      data.name = res.data.name;
    })
    .catch((error) => {
      const message = error?.response?.data?.message;
      data.loaded = true;
      
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
});
</script>

<style lang="scss" scoped></style>
