<template>
  <div class="container max-w-full w-full p-0 lg:p-5">
    <div class="sm:flex sm:items-center sm:justify-between">
      <div class="sm:flex-auto">
        <h1 class="text-3xl font-semibold text-gray-900">Product</h1>
        <p class="mt-2 text-sm text-gray-700">
        </p>
      </div>
      <div class="mt-4 sm:ml-16 sm:mt-0 sm:flex-none">
        <button @click="navigateToPage" type="button"
          class="block rounded-md bg-indigo-600 px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
          Add Product
        </button>
      </div>
    </div>

    <div class="mt-5 bg-gray-300 p-5">
      <table class="min-w-full divide-y divide-gray-300">
        <thead>
          <tr class="">
            <th scope="col" class="text-sm font-semibold text-gray-900 sm:pl-0">
              <input type="checkbox" v-model="selectAll" @change="toggleAll" class="cursor-pointer h-4 w-4">
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0">

            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0">
              ชื่อสินค้า
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0">
              รหัสสินค้า
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0">
              ประเภทสินค้า
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0">
              จำนวนสินค้า
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0">

            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-300 bg-white">
          <tr v-for="products in paginatedProduct" :key="products.p_id" @click="navigateToDetail(products)"
            class="cursor-pointer hover:bg-gray-100">
            <td class="text-center w-10">
              <input type="checkbox" v-model="products.checked" class="cursor-pointer h-4 w-4">
            </td>
            <td class="py-2 px-5 w-28 ">
              <img :src="products.image" class="w-16 border ">
            </td>
            <td class="">
              {{ products.pname }}
            </td>
            <td>
              {{ products.p_id }}
            </td>
            <td>
              {{ products.p_type }}
            </td>
            <td>
              {{ products.stock }}
            </td>
            <td class="flex relative whitespace-nowrap py-5 text-sm font-medium sm:pr-0">
              <!--edit-->
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6 mr-2">
                <path stroke-linecap="round" stroke-linejoin="round"
                  d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
              </svg>
              <!--delete-->
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6 text-red-500">
                <path stroke-linecap="round" stroke-linejoin="round"
                  d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
              </svg>

            </td>
          </tr>

        </tbody>
      </table>
    </div>

    <div class="mb-5">
      <footer class="flex items-center justify-center space-x-2 mt-4">
        <button @click="goToPage(currentPage - 1)" :disabled="currentPage === 1"
          class="px-4 py-2 bg-gray-200 text-gray-500 rounded disabled:opacity-50 disabled:cursor-not-allowed">
          Previous
        </button>
        <button v-for="page in pages" :key="page" @click="goToPage(page)"
          :class="[page === currentPage ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-500', 'px-4 py-2 rounded']">
          {{ page }}
        </button>
        <button @click="goToPage(currentPage + 1)" :disabled="currentPage === totalPages"
          class="px-4 py-2 bg-gray-200 text-gray-500 rounded disabled:opacity-50 disabled:cursor-not-allowed">
          Next
        </button>
      </footer>
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
          Description: "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image:
            "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false
        },
        {
          pname: "Pentel ดินสอกด เก็บหัวได้ เพนเทล ด้ามโลหะ 0.5mm SS465",
          p_id: "P5223643",
          brand: "Pentel",
          p_type: "เครื่องเขียน",
          stock: "20",
          Description: "มาในด้ามสวยหรู ดีไซน์สวย ทันสมัย จับถนัดมือ ด้ามสแตนเลส คุณภาพสูง ",
          image:
            "https://inwfile.com/s-fz/bukw42.jpg",
          checked: false
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
      return this.products.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.products.length / this.perPage);
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
      this.$router.push("/admin/add_product");
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