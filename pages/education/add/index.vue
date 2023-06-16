<template>
  <div class="p-10">
    <form @submit.prevent="addItem">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Education" page="add" />

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
              </div>
            </div>

            <div class="w-full grid grid-cols-2 h-min gap-10">
              <Input
                @update:value="data.major = $event"
                name="major"
                label="Major"
                placeholder="Enter major"
                :value="data.major"
              />

              <Input
                @update:value="data.place = $event"
                name="place"
                label="Place"
                placeholder="Enter place"
                :value="data.place"
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
          <SubmitBtn text="Add" />
        </div>
      </div>
    </form>
  </div>
</template>

<script setup>
import axios from "axios";
import { UploadFilled } from "@element-plus/icons-vue";
import { ElNotification } from "element-plus";

const upload = ref();

const data = reactive({
  major: "",
  place: "",
  date_from: "",
  date_to: "",
});

const formData = new FormData();

const addPhoto = (e) => {
  formData.append("images", e.raw);
};

const removePhoto = () => {
  formData.delete("images");
};

const addItem = (e) => {
  formData.append("major", data.major);
  formData.append("place", data.place);
  formData.append("date_from", data.date_from);
  formData.append("date_to", data.date_to);

  axios
    .post(`http://localhost:3001/api/education`, formData)
    .then((res) => {
      ElNotification({
        title: "Added",
        type: "success",
      });

      data.major = "";
      data.place = "";
      data.date_from = "";
      data.date_to = "";
      upload.value.clearFiles();

      formData.delete("major");
      formData.delete("place");
      formData.delete("date_from");
      formData.delete("date_to");
      formData.delete("images");

      useRouter().push({ name: "education" });
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
