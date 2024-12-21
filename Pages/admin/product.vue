<template>
  <div class="container max-w-full w-full">
    <div class="p-0 lg:p-5">
      <div class="flex sm:items-center justify-between p-5">
        <div class="sm:flex-auto">
          <h1 class="text-3xl font-semibold text-gray-900">Product</h1>
          <p class="mt-2 text-sm text-gray-700"></p>
        </div>
        <div class="sm:ml-16 sm:flex-none">
          <button
            @click="navigateToPage"
            type="button"
            class="block rounded-full bg-black px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-gray-700 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
            +
          </button>
        </div>
      </div>

      <div class="w-screen xl:w-full p-5">
        <div class="bg-gray-300 p-5 rounded-xl">
          <div class="bg-white rounded p-2 flex justify-end">
            <div class="w-80">
              <form class="">
                <div class="relative">
                  <div
                    class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
                    <svg
                      class="w-3 h-3 text-gray-500 dark:text-gray-400"
                      aria-hidden="true"
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 20 20">
                      <path
                        stroke="currentColor"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
                    </svg>
                  </div>
                  <input
                    type="search"
                    id="default-search"
                    class="block w-full p-4 ps-10 text-sm text-gray-900 border rounded-xl h-5"
                    placeholder="Search Product, ID..."
                    required />
                </div>
              </form>
            </div>
          </div>

          <div class="overflow-auto">
            <table class="min-w-full divide-y divide-gray-300 mt-5">
              <thead>
                <tr class="bg-black text-white">
                  <th
                    scope="col"
                    class="px-3 py-3.5 text-left text-sm sm:pl-0"></th>
                  <th
                    scope="col"
                    class="px-3 py-3.5 text-left text-sm font-semibold whitespace-nowrap sm:pl-0">
                    ชื่อสินค้า
                  </th>
                  <th
                    scope="col"
                    class="px-3 py-3.5 text-left text-sm font-semibold whitespace-nowrap sm:pl-0 hidden sm:table-cell">
                    รหัสสินค้า
                  </th>
                  <th
                    scope="col"
                    class="px-3 py-3.5 text-left text-sm font-semibold whitespace-nowrap sm:pl-0 hidden sm:table-cell">
                    ประเภทสินค้า
                  </th>
                  <th
                    scope="col"
                    class="px-3 py-3.5 text-left text-sm font-semibold whitespace-nowrap sm:pl-0 hidden sm:table-cell">
                    จำนวนสินค้า
                  </th>
                  <th
                    scope="col"
                    class="px-3 py-3.5 text-left text-sm font-semibold sm:pl-0"></th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-300 bg-white">
                <tr
                  v-for="products in paginatedProduct"
                  :key="products.p_id"
                  @click="navigateToDetail(products)"
                  class="cursor-pointer hover:bg-gray-100">
                  <td class="pl-2 pr-2 w-20">
                    <div class="w-10 flex justify-center">
                      <img class="" :src="products.image" alt="" />
                    </div>
                  </td>
                  <td class="truncate text-sm max-w-[200px] lg:max-w-full">
                    <div class="">
                      <div class="font-medium text-gray-900">
                        {{ products.pname }}
                      </div>
                      <div class=" text-gray-500 text-sm sm:hidden">
                        {{ products.p_id }} {{ products.p_type }}
                      </div>
                    </div>
                  </td>
                  <td class="px-4 text-sm hidden sm:table-cell">
                    {{ products.p_id }}
                  </td>
                  <td class="whitespace-nowrap text-sm px-4 hidden sm:table-cell">
                    {{ products.p_type }}
                  </td>
                  <td class="px-4 text-sm hidden sm:table-cell">
                    {{ products.stock }}
                  </td>
                  <td
                    class="flex relative whitespace-nowrap px-5 py-5 text-sm font-medium sm:pr-0">
                    <!--edit-->
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke-width="1.5"
                      stroke="currentColor"
                      class="size-6 mr-2">
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                    </svg>
                    <!--delete-->
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke-width="1.5"
                      stroke="currentColor"
                      class="size-6 text-red-500">
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                    </svg>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div>
            <div
              class="flex justify-between items-center mt-4 bg-white h-auto rounded-lg p-3 flex-col sm:flex-row ">
              <div class="">
                รายการที่ {{ startIndex + 1 }} ถึง {{ endIndex }} จากทั้งหมด
                {{ products.length }} รายการ
                <select v-model="perPage" @change="currentPage = 1">
                  <option
                    v-for="size in [5, 10, 15, 20]"
                    :key="size"
                    :value="size">
                    {{ size }} รายการต่อหน้า
                  </option>
                </select>
              </div>

              <div class="">
                <footer class="flex items-center justify-center space-x-2">
                  <button
                    @click="goToPage(currentPage - 1)"
                    :disabled="currentPage === 1"
                    class="px-2 py-1 bg-gray-200 text-gray-500 rounded disabled:opacity-50 disabled:cursor-not-allowed">
                    Previous
                  </button>
                  <button
                    v-for="page in pages"
                    :key="page"
                    @click="goToPage(page)"
                    :class="[
                      'px-2 py-1 rounded',
                      page === currentPage
                        ? 'bg-blue-500 text-white'
                        : 'bg-gray-200 text-gray-500',
                    ]">
                    {{ page }}
                  </button>
                  <button
                    @click="goToPage(currentPage + 1)"
                    :disabled="currentPage === totalPages"
                    class="px-2 py-1 bg-gray-200 text-gray-500 rounded disabled:opacity-50 disabled:cursor-not-allowed">
                    Next
                  </button>
                </footer>
              </div>
            </div>
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
      selectAll: false,
      currentPage: 1,
      perPage: 5,
      products: [],
      mockData: [
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description:
            "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image: "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false,
        },
      ],
      useApiData: false,
    };
  },
  watch: {
    products: {
      handler(newProducts) {
        this.selectAll = newProducts.every((product) => product.checked);
      },
      deep: true,
    },
  },
  computed: {
    paginatedProduct() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = this.currentPage * this.perPage;
      console.log(
        `Start: ${start}, End: ${end}, Total: ${this.products.length}`
      );
      return this.products.slice(start, end);
    },

    totalPages() {
      return Math.ceil(this.products.length / this.perPage);
    },
    startIndex() {
      return (this.currentPage - 1) * this.perPage;
    },
    endIndex() {
      return Math.min(this.startIndex + this.perPage, this.products.length);
    },
    pages() {
      return Array.from({ length: this.totalPages }, (_, i) => i + 1);
    },
  },
  methods: {
    async fetchPeopleFromApi() {
      try {
        const response = await this.$axios.get("/api/products");
        this.products = response.data;
      } catch (error) {
        console.error("Failed to fetch data from API", error);
      }
    },
    goToPage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page;
      }
    },
    navigateToPage() {
      this.$router.push("/admin/addproduct");
    },
    navigateToDetail(product) {
      this.$router.push(`/admin/productdetail/${product.p_id}`);
    },
    toggleAll() {
      this.products.forEach((product) => {
        product.checked = this.selectAll;
      });
    },
  },
  async created() {
    if (this.useApiData) {
      await this.fetchPeopleFromApi();
    } else {
      this.products = this.mockData;
    }
  },
};
</script>
