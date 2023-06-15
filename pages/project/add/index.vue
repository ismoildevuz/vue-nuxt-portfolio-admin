<template>
  <div class="p-10">
    <form @submit.prevent="addProject">
      <div class="bg-white rounded-[20px]">
        <div class="bg-[#4D44B5] px-[40px] py-[12px] rounded-t-[20px]">
          <div>
            <h3 class="text-[24px] leading-[36px] font-bold text-white">
              Info about your project
            </h3>
          </div>
        </div>

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
                  action="https://nest-portfolio-mexg.onrender.com/api/image"
                  name="images"
                  id="images"
                  list-type="picture"
                  :limit="1"
                  :auto-upload="false"
                  ref="upload"
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
          <button
            @click="error"
            type="button"
            class="group addButton py-3 px-5 flex items-center justify-center bg-[#4D44B5] rounded-full hover:bg-white border-2 border-[#4D44B5] cursor-pointer select-none"
          >
            <h5
              class="text-[18px] leading-[27px] text-white group-hover:text-[#4D44B5]"
            >
              Add
            </h5>
          </button>
          <button
            type="submit"
            class="group addButton py-3 px-5 flex items-center justify-center bg-[#4D44B5] rounded-full hover:bg-white border-2 border-[#4D44B5] cursor-pointer select-none"
          >
            <h5
              class="text-[18px] leading-[27px] text-white group-hover:text-[#4D44B5]"
            >
              Add
            </h5>
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script setup>
import { UploadFilled } from "@element-plus/icons-vue";
import { ElNotification } from "element-plus";
import axios from "axios";
const upload = ref();

const data = reactive({
  name: "",
  description: "",
  link_project: "",
  link_github: "",
});

const error = () => {
  ElNotification({
    title: "Error",
    message: "Some error",
    type: "warning",
  });
};

const formData = new FormData();

const addPhoto = (e) => {
  formData.append("images", e.raw);
};

const removePhoto = () => {
  formData.delete("images");
};

const addProject = (e) => {
  formData.append("name", data.name);
  formData.append("description", data.description);
  formData.append("link_github", data.link_github);
  formData.append("link_project", data.link_project);
  axios
    .post(`http://localhost:3001/api/project`, formData)
    .then((res) => {
      ElNotification({
        title: "Added",
        type: "success",
      });
      data.name = "";
      data.description = "";
      data.link_github = "";
      data.link_project = "";
      upload.value.clearFiles();
      formData.delete("name");
      formData.delete("description");
      formData.delete("link_github");
      formData.delete("link_project");
      formData.delete("images");
      // useRouter().push({ name: "project" });
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
