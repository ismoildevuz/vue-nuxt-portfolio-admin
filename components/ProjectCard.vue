<template>
  <nuxt-link :to="`/project/${el.id}`">
    <div
      class="bg-slate-50 rounded-xl overflow-hidden shadow-[0_2px_4px_rgba(0,0,0,0.18)] hover:shadow-[0_2px_10px_rgba(0,0,0,0.2)] duration-200 h-full flex flex-col"
    >
      <img
        width="400"
        height="200"
        class="w-[400px] h-[200px] object-cover"
        :src="`https://nest-portfolio-xy2i.onrender.com/api/image/file/${el.image?.file_name}`"
        onerror="this.src='/images/no-image-project.png'"
      />

      <div class="p-5 flex flex-col justify-between flex-grow">
        <div>
          <h2 class="text-2xl font-medium mb-4">{{ el.name }}</h2>

          <h3 class="">{{ el.description }}</h3>
        </div>

        <div class="flex justify-between items-center mt-3">
          <div class="flex items-center gap-5">
            <h6>
              <span>{{ el.views }}</span>
            </h6>

            <Rating :rating="avrage(el.rating) || 0" />
          </div>

          <div class="flex gap-2">
            <a target="_blank" :href="el.link_github">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                style="fill: rgba(0, 0, 0, 1); transform: ; msfilter: "
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M12.026 2c-5.509 0-9.974 4.465-9.974 9.974 0 4.406 2.857 8.145 6.821 9.465.499.09.679-.217.679-.481 0-.237-.008-.865-.011-1.696-2.775.602-3.361-1.338-3.361-1.338-.452-1.152-1.107-1.459-1.107-1.459-.905-.619.069-.605.069-.605 1.002.07 1.527 1.028 1.527 1.028.89 1.524 2.336 1.084 2.902.829.091-.645.351-1.085.635-1.334-2.214-.251-4.542-1.107-4.542-4.93 0-1.087.389-1.979 1.024-2.675-.101-.253-.446-1.268.099-2.64 0 0 .837-.269 2.742 1.021a9.582 9.582 0 0 1 2.496-.336 9.554 9.554 0 0 1 2.496.336c1.906-1.291 2.742-1.021 2.742-1.021.545 1.372.203 2.387.099 2.64.64.696 1.024 1.587 1.024 2.675 0 3.833-2.33 4.675-4.552 4.922.355.308.675.916.675 1.846 0 1.334-.012 2.41-.012 2.737 0 .267.178.577.687.479C19.146 20.115 22 16.379 22 11.974 22 6.465 17.535 2 12.026 2z"
                ></path>
              </svg>
            </a>

            <a target="_blank" :href="el.link_project">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                style="fill: rgba(0, 0, 0, 1); transform: ; msfilter: "
              >
                <path
                  d="M6.42 9.87 12 20.78l5.58-10.91H6.42zM5.9 4.01 2 9.24h3.62l.28-5.23zm6.1-.59L6.63 9.24h10.74L12 3.42zM2.04 9.87l9.09 10.58L5.72 9.87H2.04zM11.53 3h-.15l-4.84.51a.09.09 0 0 1 0 .05l-.27 5.15zm1.34 17.45 9.09-10.58h-3.68l-5.41 10.58zm4.58-16.87a.09.09 0 0 1 0-.05l-4-.42-1-.11 5.26 5.71zm.65.43.28 5.23H22l-2.22-2.98-1.68-2.25z"
                ></path>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </nuxt-link>
</template>

<script setup>
defineProps(["el"]);

const avrage = (rates) => {
  if (!rates.length) return 0;
  let sum = 0;
  for (let i of rates) {
    sum += i.rate;
  }
  return (sum / rates.length).toFixed(1);
};
</script>

<style lang="scss" scoped></style>
