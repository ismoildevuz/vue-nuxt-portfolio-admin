<template>
  <div v-if="data.loaded && data.item" class="p-10">
    <form @submit.prevent="updateProject">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Project" page="edit" />

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
                    :src="`http://localhost:3001/api/image/file/${data.item.image?.file_name}`"
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
                @update:value="data.description = $event"
                name="description"
                label="Description"
                placeholder="Enter description"
                :value="data.description"
              />

              <Input
                @update:value="data.link_github = $event"
                name="link_github"
                label="GitHub"
                placeholder="Enter GitHub source link"
                :value="data.link_github"
                type="url"
              />

              <Input
                @update:value="data.link_project = $event"
                name="link_project"
                label="Project"
                placeholder="Enter link to project"
                :value="data.link_project"
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
    <Error name="Project" path="project" />
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
  description: "",
  link_project: "",
  link_github: "",
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

const updateProject = (e) => {
  formData.append("name", data.name);
  formData.append("description", data.description);
  formData.append("link_github", data.link_github);
  formData.append("link_project", data.link_project);

  axios
    .patch(`http://localhost:3001/api/project/${id}`, formData)
    .then((res) => {
      ElNotification({
        title: "Updated",
        type: "success",
      });

      data.name = "";
      data.description = "";
      data.link_github = "";
      data.link_project = "";

      data.item = null;
      data.loaded = false;
      upload.value.clearFiles();

      formData.delete("name");
      formData.delete("description");
      formData.delete("link_github");
      formData.delete("link_project");
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
    .get(`http://localhost:3001/api/project/${id}`)
    .then((res) => {
      data.item = res.data;
      data.loaded = true;

      data.name = res.data.name;
      data.description = res.data.description;
      data.link_github = res.data.link_github;
      data.link_project = res.data.link_project;

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
