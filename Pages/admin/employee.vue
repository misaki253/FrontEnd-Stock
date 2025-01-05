<template>
  <div class="p-4 bg-white rounded-lg shadow-lg">
    <h1 class="text-3xl font-semibold mb-4">รายชื่อผู้ใช้งาน</h1>
    <div class="mb-4 flex space-x-4">
      <input v-model="filters.search" type="text" placeholder="ค้นหา" class="border p-2 rounded" @input="fetchUsers" />



    </div>

    <table class="min-w-full border-collapse border border-gray-200">
      <thead>
        <tr>
          <th class="border border-gray-200 px-4 py-2">No</th>
          <th class="border border-gray-200 px-4 py-2">Fristname</th>
          <th class="border border-gray-200 px-4 py-2">Lastname</th>
          <th class="border border-gray-200 px-4 py-2">Role</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="users.length === 0">
          <td colspan="5" class="text-center">ไม่พบข้อมูลผู้ใช้งาน</td>
        </tr>
        <tr v-else v-for="(user, index) in users" :key="user.userId" @click="openModal(user)"
          class="hover:bg-slate-400">
          <td class="border border-gray-200 px-4 py-2 w-10 text-center">{{ startIndex + index + 1 }}</td>
          <td class="border border-gray-200 px-4 py-2 truncate">
            {{ user.firstname }} 
          </td>
          <td class="border border-gray-200 px-4 py-2">
            {{ user.lastname }}
          </td>
          <td class="border border-gray-200 px-4 py-2 w-16 ">{{ user.role }}</td>
          

        </tr>
      </tbody>
    </table>

    <div v-if="isModalOpen" class="fixed inset-0 flex items-center justify-center bg-gray-600 bg-opacity-50 z-50">
      <div class="bg-white p-6 rounded shadow-lg w-96">
        <h2 class="text-2xl mb-4">ข้อมูลผู้ใช้</h2>
        <div class="flex justify-center p-5 mb-5">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" id="user" class="size-20">
            <ellipse cx="256" cy="130" rx="110" ry="130"></ellipse>
            <path
              d="M36 478.191C36 390.825 134.497 320 256 320s220 70.825 220 158.191C476 496.863 460.863 512 442.192 512H69.808C51.137 512 36 496.863 36 478.191z">
            </path>
          </svg>
        </div>
        <p><strong>Name:</strong> {{ edituser.firstname }} {{ edituser.lastname }}</p>
        <p><strong>Username:</strong> {{ edituser.username }}</p>
        <p><strong>Role:</strong> {{ edituser.role }}</p>

        <p><strong>สร้างเมื่อวันที่:</strong> {{ formatDate(edituser.createdDate, edituser.updatedDate) }}</p>


        <div class="flex justify-between mt-10">
          <div class="flex">
            <button @click="deleteUsers" class="bg-red-500 text-white px-4 py-2 rounded flex ">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round"
                  d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
              </svg>
              <span>ลบ</span>
            </button>
          </div>
          <button @click="closeModal" class="bg-gray-300 text-gray-700 px-4 py-2 rounded flex">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
              stroke="currentColor" class="size-6">
              <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
            </svg>

            <span>ปิด</span>
          </button>

        </div>
      </div>
    </div>

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
      isModalOpen: false,
      edituser: null
    };
  },
  computed: {
    startIndex() {
      return this.filters.page * this.filters.perpage;
    },
    endIndex() {
      const end = (this.filters.page + 1) * this.filters.perpage;
      return end > this.totalCount ? this.totalCount : end;
    },
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

    openModal(user) {
      this.edituser = { ...user };
      this.isModalOpen = true;
    },

    closeModal() {
      this.isModalOpen = false;
    },


    async saveStatus() {
      try {

        console.log(this.edituser.isUse)

        await axios.put(`https://project-stock.onrender.com/api/users/users/${this.edituser.userId}`, {
          isUse: this.edituser.isUse,
        });


        const index = this.users.findIndex(user => user.userId === this.edituser.userId);
        if (index !== -1) {
          this.users[index].isUse = this.edituser.isUse;
        }

        this.closeModal();
        console.log("Status saved successfully.");
      } catch (error) {
        console.error("Error saving status:", error);
      }
    },

    async deleteUsers() {
      if (confirm(`คุณต้องการลบผู้ใช้ ${this.edituser.firstname} ${this.edituser.lastname} หรือไม่?`)) {
        try {
          console.log("กำลังกำลังลบผู้ใช้:", this.edituser);

          // อัปเดตสถานะของผู้ใช้ให้เป็น false (ไม่ใช้งาน)
          const response = await this.updateUsersStatus(this.edituser.userId, false);

          // ตรวจสอบว่า API ทำงานสำเร็จ
          if (response.status === 200) {
            // ปิด Modal หลังจากลบผู้ใช้สำเร็จ
            this.closeModal();  // ใช้ closeModal แทน closeProductModal

            // รีเฟรชข้อมูลผู้ใช้หลังจากทำการลบ
            await this.fetchUsers();
            console.log("ผู้ใช้ถูกลบสำเร็จ");
          } else {
            throw new Error(`การลบผู้ใช้ไม่สำเร็จ: ${response.statusText}`);
          }
        } catch (error) {
          alert('ไม่สามารถลบผู้ใช้ได้ กรุณาลองใหม่อีกครั้ง');
          console.error("เกิดข้อผิดพลาดในการลบผู้ใช้:", error);
        }
      }
    },

    async updateUsersStatus(userId, isUse) {
      try {
        const response = await axios.put(
          `https://project-stock.onrender.com/api/users/users/${userId}`,
          {
            isUse: isUse,
          }
        );

        if (response.status === 200) {
          console.log('สถานะผู้ใช้งานอัปเดตเรียบร้อย');
          return response;  // ส่งคืนคำตอบของ API
        } else {
          throw new Error(`การอัปเดตสถานะผู้ใช้งานไม่สำเร็จ: ${response.statusText}`);
        }
      } catch (error) {
        console.error('เกิดข้อผิดพลาดในการอัปเดตสถานะผู้ใช้งาน:', error);
        throw error;
      }
    },
    formatDate(dateString) {
      if (!dateString) return '';
      const date = new Date(dateString);
      if (isNaN(date)) return dateString;

      const day = String(date.getDate()).padStart(2, '0');
      const month = String(date.getMonth() + 1).padStart(2, '0');
      const year = date.getFullYear();
      const hours = String(date.getHours()).padStart(2, '0');
      const minutes = String(date.getMinutes()).padStart(2, '0');
      const seconds = String(date.getSeconds()).padStart(2, '0');

      // รูปแบบวันที่ที่ต้องการ
      return `${day}-${month}-${year} ${hours}:${minutes}:${seconds}`;
    },




  },

  mounted() {
    this.fetchUsers();
  },
};
</script>
