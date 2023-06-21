<template>
  <div v-if="data.loaded && data.item" class="p-10">
    <form @submit.prevent="updateItem">
      <div class="bg-white rounded-[20px]">
        <FormHeader name="Blog" page="edit" />

        <div class="p-[40px] pb-0">
          <div class="flex gap-[24px]">
            <div class="w-[200px] flex-shrink-0 h-[600px]">
              <div class="mb-5">
                <Input
                  @update:value="data.title = $event"
                  name="title_blog"
                  label="Title"
                  placeholder="Enter title"
                  :value="data.title"
                />
              </div>

              <div class="h-[400px]">
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
                    :src="`https://nest-portfolio-xy2i.onrender.com/api/image/file/${data.item.image?.file_name}`"
                    alt=""
                  />
                </div>
              </div>
            </div>

            <div class="w-full">
              <editor
                api-key="f0tc1o55esbjszexd9h1lv61etxfc6vpojaeucnvjip9z7np"
                v-model="data.body"
                :init="{
                  menubar: false,
                  plugins: 'lists link image emoticons',
                  toolbar:
                    'anchor autolink charmap codesample emoticons image link lists media searchreplace table visualblocks wordcount checklist mediaembed casechange export formatpainter pageembed linkchecker a11ychecker tinymcespellchecker permanentpen powerpaste advtable advcode editimage tinycomments tableofcontents footnotes mergetags autocorrect typography inlinecss',
                  toolbar:
                    'undo redo | blocks fontsize | emoticons bold italic underline strikethrough  | link image media table mergetags | addcomment showcomments | spellcheckdialog a11ycheck typography | align lineheight checklist numlist bullist  | charmap | removeformat',
                }"
              />
            </div>
          </div>

          <div v-html="data.body"></div>
        </div>

        <div class="flex items-center justify-end p-10 pt-0">
          <SubmitBtn text="Save" />
        </div>
      </div>
    </form>
  </div>

  <div v-else-if="data.loaded">
    <Error name="Blog" path="blog" />
  </div>
</template>

<script setup>
import axios from "axios";
import { UploadFilled } from "@element-plus/icons-vue";
import { ElNotification } from "element-plus";
import Editor from "@tinymce/tinymce-vue";

const { id } = useRoute().params;

const upload = ref(null);
const has_image = ref(false);

const data = reactive({
  title: "",
  body: "",
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
  formData.append("title", data.title);
  formData.append("body", data.body);

  axios
    .patch(`https://nest-portfolio-xy2i.onrender.com/api/blog/${id}`, formData)
    .then((res) => {
      ElNotification({
        title: "Updated",
        type: "success",
      });

      data.title = "";
      data.body = "";

      data.item = null;
      data.loaded = false;
      upload.value.clearFiles();

      formData.delete("title");
      formData.delete("body");
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
    .get(`https://nest-portfolio-xy2i.onrender.com/api/blog/${id}`)
    .then((res) => {
      data.item = res.data;
      data.loaded = true;

      data.title = res.data.title;
      data.body = res.data.body;

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
