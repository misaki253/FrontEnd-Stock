<template>
  <div class="p-5">
    <div class="flex justify-center">
      <div class="grid grid-cols-4 gap-20">
        <div class="px-5 py-3 border rounded-xl w-70 flex items-center" v-for="n in 4" :key="n">
          <svg xmlns="http://www.w3.org/2000/svg" width="58" height="58" fill="none" viewBox="0 0 96 96" id="alert-triangle">
            <path fill="#FFB800" stroke="#000" stroke-width="5"
              d="M34.5096 17.5C40.2831 7.5 54.7169 7.5 60.4904 17.5L86.9042 63.25C92.6777 73.25 85.4608 85.75 73.9138 85.75H21.0862C9.53922 85.75 2.32234 73.25 8.09585 63.25L34.5096 17.5Z">
            </path>
            <path stroke="#000" stroke-linecap="round" stroke-linejoin="round" stroke-width="5" d="M48 43L48 54.5"></path>
            <path fill="#000"
              d="M50.5 63.1992C50.5 64.5799 49.3807 65.6992 48 65.6992C46.6193 65.6992 45.5 64.5799 45.5 63.1992C45.5 61.8185 46.6193 60.6992 48 60.6992C49.3807 60.6992 50.5 61.8185 50.5 63.1992Z">
            </path>
          </svg>
          <div class="ml-3">
            <div>
              <span class="text-xl">สินค้าใกล้หมด</span>
            </div>
            <div class="text-center">
              <span>s</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Pagination Controls -->
    <div class="bg-gray-500 mt-10 p-5 rounded-xl">
      <div>
        <div class="bg-white p-2">
          Hitory
        </div>

        <div class="mt-5">
          <table class="min-w-full divide divide-black">
            <thead>
              <tr class="divide-x bg-black text-white">
                <th class="text-left px-3 py-2.5">รหัสสินค้า</th>
                <th class="text-left px-3">ชื่อสินค้า</th>
                <th class="text-left px-3">ประเภทสินค้า</th>
                <th class="text-left px-3">จำนวน</th>
                <th class="text-left px-3">สถานะ</th>
                <th class="text-left px-3">วันที่-เวลา</th>
                <th class="text-left px-3">ผู้รับผิดชอบ</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="history in paginatedHistory" :key="history.p_id" class="divide-x bg-zinc-800 text-white">
                <td class="px-3 py-1.5">{{ history.p_id }}</td>
                <td class="px-3">{{ history.pname }}</td>
                <td class="px-3">{{ history.p_type }}</td>
                <td class="px-3">{{ history.quantity }}</td>
                <td class="px-3">{{ history.status }}</td>
                <td class="px-3">{{ history.date }}</td>
                <td class="px-3">{{ history.employee }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- Pagination Footer -->
        <div class="flex justify-between items-center mt-4 bg-white p-3">
          <!-- Total Items Display -->
          <div>
            รายการที่ {{ startIndex + 1 }} ถึง {{ endIndex }} จากทั้งหมด {{ history.length }} รายการ
            <select v-model="perPage" @change="currentPage = 1" class=" border rounded ">
              <option v-for="size in [5, 10, 15, 20]" :key="size" :value="size">
                {{ size }} รายการต่อหน้า
              </option>
            </select>
          </div>

          <!-- Page Navigation -->
          <div class="flex items-center">
            <button @click="goToPage(currentPage - 1)" :disabled="currentPage === 1" class="btn mx-1">
              &lt;
            </button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button @click="goToPage(currentPage + 1)" :disabled="currentPage === totalPages" class="btn mx-1">
              &gt;
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage: 1,
      perPage: 10, // Default items per page
      history: [],
      mockData: [
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin"
        },
        // เพิ่ม mock data ตามต้องการ
      ],
      useApiData: false,
    };
  },
  computed: {
    paginatedHistory() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = this.currentPage * this.perPage;
      return this.history.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.history.length / this.perPage);
    },
    startIndex() {
      return (this.currentPage - 1) * this.perPage;
    },
    endIndex() {
      return Math.min(this.startIndex + this.perPage, this.history.length);
    }
  },
  methods: {
    async fetchPeopleFromApi() {
      try {
        const response = await this.$axios.get("/api/history");
        this.history = response.data;
      } catch (error) {
        console.error("Failed to fetch data from API", error);
      }
    },
    goToPage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page;
      }
    },
  },
  mounted() {
    if (this.useApiData) {
      this.fetchPeopleFromApi();
    } else {
      this.history = this.mockData;
    }
  }
};
</script>
