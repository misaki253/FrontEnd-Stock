<template>
  <div class="p-4 bg-white rounded-lg shadow-lg">
    <h1 class="text-3xl font-semibold mb-4">รายการสินค้า</h1>
    <div class="mb-4 flex space-x-4">
      <input v-model="filters.search" type="text" placeholder="ค้นหา" class="border p-2 rounded" @input="fetchUsers" />



    </div>

    <table class="min-w-full border-collapse border border-gray-200">
      <thead>
        <tr>
          <th class="border border-gray-200 px-4 py-2">No</th>
          <th class="border border-gray-200 px-4 py-2">Name</th>
          <th class="border border-gray-200 px-4 py-2">Role</th>
          <th class="border border-gray-200 px-4 py-2"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="users.length === 0">
          <td colspan="5" class="text-center">ไม่พบข้อมูลผู้ใช้งาน</td>
        </tr>
        <tr v-else v-for="(user, index) in users" :key="user.userId"
          class="hover:bg-slate-400">
          <td class="border border-gray-200 px-4 py-2">{{ startIndex + index + 1 }}</td>
          <td class="border border-gray-200 px-4 py-2">
            {{ user.firstname }}   {{ user.lastname }}
          </td>
          <td class="border border-gray-200 px-4 py-2">{{ user.role }}</td>
          <td class="border border-gray-200 px-4 py-2"></td>

        </tr>
      </tbody>


    </table>

    <div class="flex justify-between mt-5">
      <div class="flex  items-center">
        <p class="p-3">
          รายการที่ {{ startIndex + 1 }} ถึง {{ endIndex }} จากทั้งหมด
          {{ totalCount }} รายการ แสดง
        </p>

        <select v-model="filters.perpage" @change="changePage(0)" class="p-2 border rounded">
          <option v-for="size in [5, 10, 15, 20]" :key="size" :value="size">
            {{ size }} รายการต่อหน้า
          </option>
        </select>
      </div>

      <div class="flex items-center justify-end space-x-2">
        <!-- ปุ่มย้อนกลับ -->
        <button @click="changePage(currentPage - 1)" :disabled="currentPage === 0"
          class="p-2 bg-gray-300 text-black rounded">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="size-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" />
          </svg>
        </button>

        <!-- ปุ่มแสดงหมายเลขหน้า -->
        <button v-for="page in totalPages" :key="page" @click="changePage(page - 1)"
          :class="{ 'bg-blue-500 text-white': currentPage === page - 1, 'bg-white text-black': currentPage !== page - 1 }"
          class="px-2 py-1 rounded">
          {{ page }}
        </button>

        <!-- ปุ่มไปข้างหน้า -->
        <button @click="changePage(currentPage + 1)" :disabled="currentPage + 1 >= totalPages"
          class="p-2 bg-gray-300 text-black rounded">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="size-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="m8.25 4.5 7.5 7.5-7.5 7.5" />
          </svg>
        </button>
      </div>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      users: [],
      currentPage: 0,
      totalPages: 0,
      isLoading: false,
      filters: {
        page: 0,
        perpage: 5,
        isuse: true,
        max: "desc",
        search: "",
      },
    };
  },
  computed: {
    startIndex() {
      return this.filters.page * this.filters.perpage;
    },
    displayItems() {
      return this.items.slice(this.startIndex, this.startIndex + this.filters.perpage);
    },
    endIndex() {
      const end = (this.filters.page + 1) * this.filters.perpage;
      return end > this.totalCount ? this.totalCount : end;
    }
  },
  methods: {

    async fetchUsers() {
      try {

        this.users = [];
        const response = await axios.post("https://project-stock.onrender.com/api/users/users", this.filters, {
          headers: {
            "Content-Type": "application/json",
          },
        });


        const { data, totalCount } = response.data;

        this.users = data;
        this.totalCount = totalCount;
        this.totalPages = Math.ceil(this.totalCount / this.filters.perpage);

        if (isNaN(this.totalPages) || this.totalPages <= 0) {
          this.totalPages = 1;
        }
      } catch (error) {
        console.error("Error fetching users:", error);
        this.users = [];
        this.totalCount = 0;
        this.totalPages = 1;
      }
    },


    changePage(page) {
      if (page >= 0 && page < this.totalPages) {
        this.currentPage = page;
        this.filters.page = page;
        this.fetchUsers();
      }
    },
  },

  mounted() {
    this.fetchUsers();
  },
};
</script>
