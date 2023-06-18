<template>
  <div v-if="data.loaded && data.item" class="p-5">
    <div
      class="bg-slate-50 rounded-xl overflow-hidden shadow-[0_2px_4px_rgba(0,0,0,0.18)] duration-200 h-full flex flex-col"
    >
      <div class="relative">
        <nuxt-link
          :to="`/blog/edit/${data.item.id}`"
          class="absolute top-5 right-5 bg-[#343541] rounded-full p-2"
        >
          <Icons name="create" color="#ACACBE" />
        </nuxt-link>

        <button
          @click="open"
          class="absolute bottom-5 right-5 bg-[#343541] rounded-full p-2"
        >
          <Icons name="delete_outline" color="#ACACBE" />
        </button>

        <img
          height="400"
          class="w-full h-[400px] object-cover select-none"
          :src="`http://localhost:3001/api/image/file/${data.item.image?.file_name}`"
          onerror="this.src='/images/no-image-blog.png'"
        />
      </div>

      <div class="p-5 flex flex-col justify-between flex-grow">
        <div>
          <h2 class="text-2xl font-medium mb-4">{{ data.item.title }}</h2>

          <div v-html="data.item.body"></div>
        </div>

        <div class="flex justify-between items-end mt-3">
          <div class="flex gap-5">
            <h6>
              <span>{{ data.item.views }} views</span>
            </h6>
          </div>

          <div class="flex items-center gap-5">
            <h6 class="text-sm text-gray-500">
              <span>{{
                moment(data.item.createdAt).format("YYYY.MM.DD")
              }}</span>
            </h6>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div v-else-if="data.loaded">
    <Error name="Blog" path="blog" />
  </div>
</template>

<script setup>
import axios from "axios";
import moment from "moment";
import { ElNotification, ElMessageBox } from "element-plus";

const { id } = useRoute().params;

const data = reactive({
  item: null,
  loaded: false,
});

const open = () => {
  ElMessageBox.confirm(
    "This action will delete the Blog. Continue?",
    "Warning",
    {
      confirmButtonText: "OK",
      cancelButtonText: "Cancel",
      type: "warning",
    }
  )
    .then(() => {
      axios
        .delete(`http://localhost:3001/api/blog/${id}`)
        .then((res) => {
          ElNotification({
            title: "Deleted",
            type: "success",
          });

          useRouter().push({ name: "blog" });
        })
        .catch((error) => {
          const message = error?.response?.data?.message;
          console.log(error);
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
    })
    .catch(() => {});
};

onMounted(() => {
  axios
    .get(`http://localhost:3001/api/blog/${id}`)
    .then((res) => {
      data.item = res.data;
      data.loaded = true;
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
