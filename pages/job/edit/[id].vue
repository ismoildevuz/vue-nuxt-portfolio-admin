<template>
  <div v-if="data.loaded && data.item" class="p-10">
    <form @submit.prevent="updateItem">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Job" page="edit" />

        <div class="p-[40px] pb-0">
          <div class="flex gap-[24px]">
            <div class="w-[200px] flex-shrink-0 h-[400px]">
              <div>
                <label
                  for="image"
                  class="text-[18px] leading-[27px] font-semibold text-[#303972] mb-[16px] block"
                  >Image *</label
                >
                <el-upload
                  @change="addPhoto"
                  @remove="removePhoto"
                  class="upload-demo"
                  drag
                  name="images"
                  id="images"
                  list-type="picture"
                  :limit="1"
                  :auto-upload="false"
                  ref="upload"
                  accept=".jpg, .png"
                >
                  <el-icon class="el-icon--upload"><upload-filled /></el-icon>
                  <div class="el-upload__text">
                    Drop file here or <em>click to upload</em>
                  </div>
                  <template #tip>
                    <div class="el-upload__tip">
                      jpg/png files with a size less than 500kb
                    </div>
                  </template>
                </el-upload>

                <div :class="`${has_image ? 'block' : 'hidden'} duration-1000`">
                  <img
                    width="50"
                    height="50"
                    class="w-[50px] h-[50px] object-cover rounded-full"
                    :src="`https://nest-portfolio-xy2i.onrender.com/api/image/file/${data.item.image?.file_name}`"
                    alt=""
                  />
                </div>
              </div>
            </div>

            <div class="w-full grid grid-cols-2 h-min gap-10">
              <Input
                @update:value="data.position = $event"
                name="position"
                label="Position"
                placeholder="Enter position"
                :value="data.position"
              />

              <Input
                @update:value="data.company = $event"
                name="company"
                label="Company"
                placeholder="Enter company"
                :value="data.company"
              />

              <Input
                @update:value="data.date_from = $event"
                name="date_from"
                label="Date from"
                placeholder="Enter date from"
                :value="data.date_from"
                type="date"
              />

              <Input
                @update:value="data.date_to = $event"
                name="date_to"
                label="Date to"
                placeholder="Enter date to"
                :value="data.date_to"
                type="date"
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
    <Error name="Job" path="job" />
  </div>
</template>

<script setup>
import axios from "axios";
import { UploadFilled } from "@element-plus/icons-vue";
import { ElNotification } from "element-plus";
import moment from "moment";

const { id } = useRoute().params;

const upload = ref(null);
const has_image = ref(false);

const data = reactive({
  position: "",
  company: "",
  date_from: "",
  date_to: "",
  item: null,
  loaded: false,
});

const formData = new FormData();

const addPhoto = (e) => {
  has_image.value = false;
  formData.append("images", e.raw);
};

const removePhoto = () => {
  has_image.value = true;
  formData.delete("images");
};

const updateItem = (e) => {
  formData.append("position", data.position);
  formData.append("company", data.company);
  formData.append("date_from", data.date_from);
  formData.append("date_to", data.date_to);

  axios
    .patch(`https://nest-portfolio-xy2i.onrender.com/api/job/${id}`, formData)
    .then((res) => {
      ElNotification({
        title: "Updated",
        type: "success",
      });

      data.position = "";
      data.company = "";
      data.date_from = "";
      data.date_to = "";

      data.item = null;
      data.loaded = false;
      upload.value.clearFiles();

      formData.delete("position");
      formData.delete("company");
      formData.delete("date_from");
      formData.delete("date_to");
      formData.delete("images");

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
    .get(`https://nest-portfolio-xy2i.onrender.com/api/job/${id}`)
    .then((res) => {
      data.item = res.data;
      data.loaded = true;

      data.position = res.data.position;
      data.company = res.data.company;
      data.date_from = moment(res.data.date_from).format("YYYY-MM-DD");
      data.date_to = moment(res.data.date_to).format("YYYY-MM-DD");

      if (res.data.image?.file_name) {
        has_image.value = true;
      }
    })
    .catch((error) => {
      const message = error?.response?.data?.message;
      data.loaded = true;
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
});
</script>

<style lang="scss" scoped></style>
