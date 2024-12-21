<template>
  <div class="">
    <div class="rounded-xl px-5 w-screen sm:w-full">
      <div class="bg-gray-500 p-5 rounded">
        <div class="bg-white p-2">สถานะสินค้าลงคลัง</div>
        <div class="mt-5">
          <div class="overflow-auto h-[250px]">
            <table class="min-w-full table-auto">
              <thead>
                <tr class="divide-x divide-gray-700 bg-black text-white">
                  <th class="text-left text-xs px-3 py-2.5">รหัสสินค้า</th>
                  <th class="text-left text-xs px-3">ชื่อสินค้า</th>
                  <th class="text-left text-xs px-3">ประเภทสินค้า</th>
                  <th class="text-left text-xs px-3">จำนวน</th>
                  <th class="text-left text-xs px-3">วันที่-เวลา</th>
                  <th class="text-left text-xs px-3">ผู้รับผิดชอบ</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-700">
                <tr
                  v-for="history in mockData"
                  :key="history.p_id"
                  class="divide-x divide-gray-700 bg-zinc-800 text-white">
                  <td class="px-3 py-1.5 text-xs">{{ history.p_id }}</td>
                  <td class="px-3 truncate max-w-[200px] text-xs">
                    {{ history.pname }}
                  </td>
                  <td class="whitespace-pre px-3 text-xs">
                    {{ history.p_type }}
                  </td>
                  <td class="px-3 text-xs">{{ history.quantity }}</td>
                  <td class="px-3 text-xs">{{ history.date }}</td>
                  <td class="px-3 text-xs">{{ history.employee }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="bg-gray-500 p-5 rounded mt-5">
        <div class="bg-white p-2">สถานะสินค้าลงคลัง</div>

        <div class="mt-5">
          <div class="overflow-auto h-[250px]">
            <table class="min-w-full table-auto">
              <thead>
                <tr class="divide-x divide-gray-700 bg-black text-white">
                  <th class="text-left text-xs px-3 py-2.5">รหัสสินค้า</th>
                  <th class="text-left text-xs px-3">ชื่อสินค้า</th>
                  <th class="text-left text-xs px-3">ประเภทสินค้า</th>
                  <th class="text-left text-xs px-3">จำนวน</th>
                  <th class="text-left text-xs px-3">วันที่-เวลา</th>
                  <th class="text-left text-xs px-3">ผู้รับผิดชอบ</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-700">
                <tr
                  v-for="history in mockData"
                  :key="history.p_id"
                  class="divide-x divide-gray-700 bg-zinc-800 text-white">
                  <td class="px-3 py-1.5 text-xs">{{ history.p_id }}</td>
                  <td class="px-3 truncate max-w-[200px] text-xs">
                    {{ history.pname }}
                  </td>
                  <td class="whitespace-pre px-3 text-xs">
                    {{ history.p_type }}
                  </td>
                  <td class="px-3 text-xs">{{ history.quantity }}</td>
                  <td class="px-3 text-xs">{{ history.date }}</td>
                  <td class="px-3 text-xs">{{ history.employee }}</td>
                </tr>
              </tbody>
            </table>
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
      perPage: 5,
      history: [],
      mockData: [
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
        {
          p_id: "P5223643",
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_type: "เครื่องเขียน",
          quantity: "20",
          status: "นำเข้า",
          date: "18/12/2567 22:26",
          employee: "Admin",
        },
      ],
      useApiData: false,
    };
  },
  // computed: {
  //   paginatedHistory() {
  //     const start = (this.currentPage - 1) * this.perPage;
  //     const end = this.currentPage * this.perPage;
  //     return this.history.slice(start, end);
  //   },
  //   totalPages() {
  //     return Math.ceil(this.history.length / this.perPage);
  //   },
  //   startIndex() {
  //     return (this.currentPage - 1) * this.perPage;
  //   },
  //   endIndex() {
  //     return Math.min(this.startIndex + this.perPage, this.history.length);
  //   },
  //   pages() {
  //     return Array.from({ length: this.totalPages }, (_, i) => i + 1);
  //   },
  // },
  methods: {
    async fetchPeopleFromApi() {
      try {
        const response = await this.$axios.get("/api/history");
        this.history = response.data;
      } catch (error) {
        console.error("Failed to fetch data from API", error);
      }
    },
    // goToPage(page) {
    //   if (page >= 1 && page <= this.totalPages) {
    //     this.currentPage = page;
    //   }
    // },
  },
  mounted() {
    if (this.useApiData) {
      this.fetchPeopleFromApi();
    } else {
      this.history = this.mockData;
    }
  },
};
</script>
