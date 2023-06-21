<template>
  <div>
    <div
      class="bg-slate-50 rounded-full overflow-hidden shadow-[0_2px_4px_rgba(0,0,0,0.18)] duration-200 h-full flex items-center justify-between"
    >
      <div class="flex items-center gap-5">
        <img
          width="100"
          height="100"
          class="w-[100px] h-[100px] object-cover rounded-full m-2"
          :src="`https://nest-portfolio-xy2i.onrender.com/api/image/file/${el.image?.file_name}`"
          onerror="this.src='/images/no-image-edu.png'"
        />

        <div class="">
          <div class="flex gap-5 items-end">
            <h3 class="text-sm">
              <span>{{ el.place }}</span>
            </h3>

            <h4 class="text-xs text-slate-600">
              <span>
                {{ moment(el.date_from).format("YYYY.MM.DD") }} -
                {{ moment(el.date_to).format("YYYY.MM.DD") }}
              </span>
            </h4>
          </div>

          <h2 class="text-2xl mt-2 font-medium">
            <span>{{ el.major }}</span>
          </h2>
        </div>
      </div>

      <div class="flex items-center gap-5 m-2 mr-5">
        <nuxt-link
          :to="`/education/edit/${el.id}`"
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
import moment from "moment";
import { ElNotification, ElMessageBox } from "element-plus";

defineProps(["el"]);
const emits = defineEmits(["update-list"]);

const open = (id) => {
  ElMessageBox.confirm(
    "This action will delete the Education. Continue?",
    "Warning",
    {
      confirmButtonText: "OK",
      cancelButtonText: "Cancel",
      type: "warning",
    }
  )
    .then(() => {
      axios
        .delete(`https://nest-portfolio-xy2i.onrender.com/api/education/${id}`)
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
