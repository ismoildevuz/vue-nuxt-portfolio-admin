<template>
  <nuxt-link :to="`/blog/${el.id}`">
    <div
      class="bg-slate-50 rounded-xl overflow-hidden shadow-[0_2px_4px_rgba(0,0,0,0.18)] hover:shadow-[0_2px_10px_rgba(0,0,0,0.2)] duration-200 h-full flex flex-col"
    >
      <img
        width="400"
        height="200"
        class="w-[400px] h-[200px] object-cover"
        :src="`https://nest-portfolio-xy2i.onrender.com/api/image/file/${el.image?.file_name}`"
        onerror="this.src='/images/no-image-blog.png'"
      />
      <div class="p-5 flex flex-col justify-between flex-grow">
        <div>
          <h2 class="text-2xl font-medium mb-4">{{ el.title }}</h2>

          <div v-html="format(el.body)" class="h-[100px] overflow-hidden"></div>
        </div>

        <div class="flex justify-between items-center mt-3">
          <div class="flex items-center gap-5">
            <h6>
              <span>{{ el.views }}</span>
            </h6>
          </div>

          <div class="flex items-center gap-5">
            <h6 class="text-sm text-gray-500">
              <span>{{ moment(el.createdAt).format("YYYY.MM.DD") }}</span>
            </h6>
          </div>
        </div>
      </div>
    </div>
  </nuxt-link>
</template>

<script setup>
import DOMPurify from "dompurify";
import moment from "moment";

defineProps(["el"]);

const format = (html) => {
  let sanitizedHTML = DOMPurify.sanitize(html, {
    FORBID_TAGS: ["style", "script", "img"],
    FORBID_ATTR: ["width", "height", "style"],
  });
  sanitizedHTML = sanitizedHTML.replace(/&nbsp;/g, "");
  return sanitizedHTML;
};
</script>

<style lang="scss" scoped></style>
