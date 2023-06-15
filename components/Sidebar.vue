<template>
  <aside class="w-[245px] bg-[#4D44B5] h-screen fixed top-0 left-0">
    <nuxt-link
      to="/"
      class="flex w-[145px] justify-between mx-[50px] my-[30px]"
    >
      <h3 class="text-[36px] leading-[54px] font-bold text-white">Porfolio</h3>
    </nuxt-link>

    <ul class="ml-[50px]">
      <li v-for="el in navlinks" :key="el.id">
        <NavItem
          :link="el.link"
          :title="el.title"
          :icon="el.icon"
          :id="el.id"
        />
      </li>
    </ul>
  </aside>
</template>

<script setup>
const navlinks = ref([
  {
    id: 7,
    title: "Home",
    link: "/",
    icon: "dashboard",
  },
  {
    id: 1,
    title: "Project",
    link: "/project",
    icon: "folder_copy",
  },
  {
    id: 2,
    title: "Blog",
    link: "/bLog",
    icon: "textsms",
  },
  {
    id: 3,
    title: "Skill",
    link: "/skill",
    icon: "star_border",
  },
  {
    id: 4,
    title: "Job",
    link: "/job",
    icon: "business",
  },
  {
    id: 5,
    title: "Education",
    link: "/education",
    icon: "desktop_mac",
  },
  {
    id: 6,
    title: "Social Media",
    link: "/social-media",
    icon: "language",
  },
]);
</script>

<style lang="scss" scoped></style>
