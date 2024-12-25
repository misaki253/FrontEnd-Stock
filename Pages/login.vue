<template>
  <div class="flex justify-center items-center h-screen bg-gray-100 p-5 sm:p-5">
    <div class="w-96 bg-white shadow-md rounded-lg p-6">
      <h2 class="text-2xl font-bold mb-4 text-center">
        {{ isLogin ? "Login" : "Sign Up" }}
      </h2>

      <form v-if="isLogin" @submit.prevent="Login">
        <div class="mb-4">
          <label class="block text-gray-700">Username</label>
          <input
            type="text"
            v-model="users.username"
            class="w-full px-4 py-2 border rounded-md"
            placeholder="Enter your username" />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Password</label>
          <input
            type="password"
            v-model="users.password"
            class="w-full px-4 py-2 border rounded-md"
            placeholder="Enter your password" />
        </div>
        <button
          type="submit"
          class="w-full bg-blue-500 text-white py-2 rounded-md">
          Login
        </button>
      </form>
      
      <form v-else @submit.prevent="register">
        <div class="mb-4">
          <label class="block text-gray-700">First Name</label>
          <input
            type="text"
            v-model="registerData.firstname"
            class="w-full px-4 py-2 border rounded-md"
            placeholder="Enter your first name"
            required />
        </div>

        <div class="mb-4">
          <label class="block text-gray-700">Last Name</label>
          <input
            type="text"
            v-model="registerData.lastname"
            class="w-full px-4 py-2 border rounded-md"
            placeholder="Enter your last name"
            required />
        </div>

        <div class="mb-4">
          <label class="block text-gray-700">Username</label>
          <input
            type="text"
            v-model="registerData.username"
            class="w-full px-4 py-2 border rounded-md"
            placeholder="Enter your username"
            required />
        </div>

        <div class="mb-4">
          <label class="block text-gray-700">Password</label>
          <input
            type="password"
            v-model="registerData.password"
            class="w-full px-4 py-2 border rounded-md"
            placeholder="Enter your password"
            required />
        </div>

        <button
          type="submit"
          class="w-full bg-green-500 text-white py-2 rounded-md">
          Sign Up
        </button>
      </form>

      <p class="text-center mt-4">
        {{ isLogin ? "Don't have an account?" : "Already have an account?" }}
        <button @click="isLogin = !isLogin" class="text-blue-500 underline">
          {{ isLogin ? "Sign Up" : "Login" }}
        </button>
      </p>
    </div>
    <div v-if="errorMessage" class="text-red-500 text-center mt-2">
  {{ errorMessage }}
</div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: false,
});
</script>

<script>
import axios from "axios";

export default {
  data() {
  return {
    isLogin: true,
    users: {
      username: "",
      password: "",
    },
    registerData: {
      username: "",
      password: "",
      firstname: "",
      lastname: "",
      role: "employee",
    },
    clearregisterData: {
      username: "",
      password: "",
      firstname: "",
      lastname: "",
      role: "employee",
    },
    errorMessage: "",
  };
},

  methods: {
    Login() {
      if (!this.users.username || !this.users.password) {
        alert("กรุณากรอก username และ password");
        return;
      }

      const existingUsers = JSON.parse(localStorage.getItem("users")) || [];
      const user = existingUsers.find(
        (u) =>
          u.username === this.users.username &&
          u.password === this.users.password
      );

      if (user) {
        alert("Login สำเร็จ");
        this.$router.push("/admin/homepage");
      } else {
        alert("Login ไม่สำเร็จ");
      }
    },

    async register() {
  try {
    const { data } = await axios.post(
      "http://localhost:3000/api/register",
      this.registerData
    );
    if (data) {
      this.$router.push("/login");
      this.registerData = { ...this.clearregisterData };
    }
  } catch (error) {
    console.log(error);
    if (error.response && error.response.data) {
      this.errorMessage = error.response.data.message || "เกิดข้อผิดพลาดในการลงทะเบียน";
    }
  }
},

  },
};
</script>

<style scoped>
/* ใช้ Tailwind CSS */
</style>
