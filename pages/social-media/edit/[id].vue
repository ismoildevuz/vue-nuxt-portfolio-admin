<template>
  <div v-if="data.loaded && data.item" class="p-10">
    <form @submit.prevent="updateItem">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Social Media" page="edit" />

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
                  name="image"
                  id="image"
                  list-type="picture"
                  :limit="1"
                  :auto-upload="false"
                  ref="upload"
                  accept=".jpeg, .jpg, .png"
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
                    :src="`https://nest-portfolio-xy2i.onrender.com/api/image/${data.item.image_name}`"
                    alt=""
                  />
                </div>
              </div>
            </div>

            <div class="w-full grid grid-cols-2 h-min gap-10">
              <Input
                @update:value="data.name = $event"
                name="name"
                label="Name"
                placeholder="Enter name"
                :value="data.name"
              />

              <Input
                @update:value="data.link = $event"
                name="link"
                label="Link"
                placeholder="Enter link"
                :value="data.link"
                type="url"
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
    <Error name="Social Media" path="social-media" />
  </div>
</template>

<script setup>
import axios from "axios";
import { UploadFilled } from "@element-plus/icons-vue";
import { ElNotification } from "element-plus";

const { id } = useRoute().params;

const upload = ref(null);
const has_image = ref(false);

const data = reactive({
  name: "",
  link: "",
  item: null,
  loaded: false,
});

const formData = new FormData();

const addPhoto = (e) => {
  has_image.value = false;
  formData.append("image", e.raw);
};

const removePhoto = () => {
  has_image.value = true;
  formData.delete("image");
};

const updateItem = (e) => {
  formData.append("name", data.name);
  formData.append("link", data.link);

  axios
    .patch(`https://nest-portfolio-xy2i.onrender.com/api/social-media/${id}`, formData)
    .then((res) => {
      ElNotification({
        title: "Updated",
        type: "success",
      });

      data.name = "";
      data.link = "";

      data.item = null;
      data.loaded = false;
      upload.value.clearFiles();

      formData.delete("name");
      formData.delete("link");
      formData.delete("image");

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
    .get(`https://nest-portfolio-xy2i.onrender.com/api/social-media/${id}`)
    .then((res) => {
      data.item = res.data;
      data.loaded = true;

      data.name = res.data.name;
      data.link = res.data.link;

      if (res.data.image_name) {
        has_image.value = true;
      }
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
