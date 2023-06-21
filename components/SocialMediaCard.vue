<template>
  <div>
    <div
      class="bg-slate-50 rounded-full overflow-hidden shadow-[0_2px_4px_rgba(0,0,0,0.18)] duration-200 h-full flex items-center justify-between"
    >
      <a target="_blank" :href="el.link">
        <div class="flex items-center gap-5">
          <img
            width="50"
            height="50"
            class="w-[50px] h-[50px] object-cover rounded-full m-2"
            :src="`https://nest-portfolio-xy2i.onrender.com/api/image/file/${el.image?.file_name}`"
            onerror="this.src='/images/no-image-sm.png'"
          />

          <h3>
            <span>{{ el.name }}</span>
          </h3>
        </div>
      </a>

      <div class="flex items-center gap-5 m-2">
        <nuxt-link
          :to="`/social-media/edit/${el.id}`"
          class="bg-[#e8e8e8] hover:bg-[#d6d5d5] rounded-full p-2"
        >
          <Icons name="create" color="#ACACBE" />
        </nuxt-link>

        <button
          @click="open(el.id)"
          class="bg-[#e8e8e8] hover:bg-[#d6d5d5] rounded-full p-2"
        >
          <Icons name="delete_outline" color="#ACACBE" />
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ElNotification, ElMessageBox } from "element-plus";

defineProps(["el"]);
const emits = defineEmits(["update-list"]);

const open = (id) => {
  ElMessageBox.confirm(
    "This action will delete the Social Media Link. Continue?",
    "Warning",
    {
      confirmButtonText: "OK",
      cancelButtonText: "Cancel",
      type: "warning",
    }
  )
    .then(() => {
      axios
        .delete(`https://nest-portfolio-xy2i.onrender.com/api/social-media/${id}`)
        .then((res) => {
          ElNotification({
            title: "Deleted",
            type: "success",
          });

          emits("update-list");
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
            console.log(error);
            ElNotification({
              title: "Error",
              message: message,
              type: "warning",
            });
          }
        });
    })
    .catch(() => {});
};
</script>

<style lang="scss" scoped></style>
