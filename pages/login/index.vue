<template>
  <div class="flex items-center justify-center h-screen">
    <div class="w-[430px] p-5 rounded-lg">
      <form @submit.prevent="login">
        <h1 class="font-bold text-[30px] leading-[28px] mb-5">Log in</h1>

        <input
          v-model="data.email"
          type="email"
          name="email"
          id="email"
          placeholder="Email address"
          class="w-full p-3 my-2 border border-[#00000033] rounded-md focus:outline-none"
          required
        />

        <input
          v-model="data.password"
          type="password"
          name="password"
          id="password"
          placeholder="Password"
          class="w-full p-3 my-2 border border-[#00000033] rounded-md focus:outline-none"
          required
        />

        <button
          type="submit"
          class="w-full p-4 my-5 flex items-center justify-center text-white font-bold text-[14px] leading-[18px] bg-[#1DA1F2] rounded-full"
        >
          <span>Log in</span>
        </button>

        <div class="flex justify-between">
          <button type="button" class="text-[#1DA1F2] my-2">
            <span>Forgot password?</span>
          </button>

          <button
            type="button"
            class="text-[#1DA1F2] my-2"
            @click="() => useRouter().push({ name: 'signup' })"
          >
            <span>Sign up</span>
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ElNotification } from "element-plus";
import axios from "axios";

definePageMeta({
  layout: "auth",
});

const data = reactive({
  email: "",
  password: "",
});

const login = () => {
  const admin = {
    email: data.email,
    password: data.password,
  };

  axios
    .post(`https://nest-portfolio-xy2i.onrender.com/api/admin/auth/signin`, admin)
    .then((res) => {
      ElNotification({
        title: "Signed in",
        type: "success",
        position: "bottom-right",
      });
      data.email = "";
      data.password = "";
      localStorage.setItem("token", res?.data?.data?.token);
      useRouter().push({ name: "index" });
    })
    .catch((error) => {
      const message = error?.response?.data?.message;
      if (typeof message == "object") {
        for (let i of message) {
          ElNotification({
            title: "Error",
            message: i,
            type: "warning",
            position: "bottom-right",
          });
        }
      } else {
        ElNotification({
          title: "Error",
          message: message,
          type: "warning",
          position: "bottom-right",
        });
      }
    });
};
</script>

<style lang="scss" scoped></style>
