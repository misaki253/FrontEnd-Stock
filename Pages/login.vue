<template>
    <div class="flex justify-center items-center h-screen bg-gray-100">
      <div class="w-96 bg-white shadow-md rounded-lg p-6">
        <h2 class="text-2xl font-bold mb-4 text-center">
          {{ isLogin ? "Login" : "Sign Up" }}
        </h2>
  
        <form v-if="isLogin" @submit.prevent="Login">
          <div class="mb-4">
            <label class="block text-gray-700">Username</label>
            <input
              type="username"
              v-model="users.username"
              class="w-full px-4 py-2 border rounded-md"
              placeholder="Enter your username"
            />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700">Password</label>
            <input
              type="password"
              v-model="users.password"
              class="w-full px-4 py-2 border rounded-md"
              placeholder="Enter your password"
            />
          </div>
          <button
            type="submit"
            class="w-full bg-blue-500 text-white py-2 rounded-md"
          >
            Login
          </button>
        </form>
  
        <form v-else @submit.prevent="Signup">
          <div class="mb-4">
            <label for="name" class="block text-gray-700">Name</label>
            <input
              type="text"
              v-model="users.uname"
              class="w-full px-4 py-2 border rounded-md"
              placeholder="Enter your name"
              required
            />
          </div>
  
          <div class="mb-4">
            <label for="email" class="block text-gray-700">Email</label>
            <input
              type="email"
              v-model="users.email"
              class="w-full px-4 py-2 border rounded-md"
              placeholder="Enter your email"
              required
            />
          </div>
          <div class="mb-4">
            <label for="username" class="block text-gray-700">Username</label>
            <input
              type="username"
              v-model="users.username"
              class="w-full px-4 py-2 border rounded-md"
              placeholder="Enter your username"
              required
            />
          </div>
          <div class="mb-4">
            <label for="password" class="block text-gray-700">Password</label>
            <input
              type="password"
              v-model="users.password"
              class="w-full px-4 py-2 border rounded-md"
              placeholder="Enter your password"
              required
            />
          </div>
          <button
            type="submit"
            class="w-full bg-green-500 text-white py-2 rounded-md"
          >
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
    </div>
  </template>
  
  <script>
  export default {
    layout: "login",
    data() {
      return {
        isLogin: true,
        users: {
          uname: "",
          email: "",
          username: "",
          password: "",
        },
      };
    },
  
    methods: {
      Login() {
        const existingUsers = JSON.parse(localStorage.getItem("users")) || [];
  
        const user = existingUsers.find(
          (u) =>
            u.username === this.users.username &&
            u.password === this.users.password
        );
  
        if (user) {
          console.log("Login successful for", user);
          alert("Login สำเร็จ");
          this.$router.push("/homepage"); 
        } else {
          console.log("Invalid username or password");
          alert("login ไม่สำเร็จ");
        }
      },
  
      Signup() {
        let existingUsers = JSON.parse(localStorage.getItem("users")); // ดึงข้อมูลจาก localStorage
  
        
        if (!Array.isArray(existingUsers)) {
          existingUsers = [];
        }
  
        
        const newuser = {
          ...this.users,
          id: new Date().getTime(),
        };
  
        
        existingUsers.push(newuser);
  
        
        localStorage.setItem("users", JSON.stringify(existingUsers));
  
        console.log("Sign Up successful for", newuser);
        alert("สมัครสมาชิกสำเร็จ");
        this.isLogin = true; 
      },
    },
  };
  </script>
  
  <style>
  /* ใช้ Tailwind CSS */
  </style>
  