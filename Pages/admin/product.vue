<template>
  <div>
    <h1 class="text-3xl font-semibold mb-4">Product List</h1>
    <div class="mb-4 flex space-x-4">
      <input
        v-model="filters.search"
        type="text"
        placeholder="Search"
        class="border p-2 rounded"
        @input="fetchProducts" />
      <select
        v-model="filters.productType"
        class="border p-2 rounded"
        @change="fetchProducts">
        <option value="">All Types</option>
        <option v-for="type in productType" :key="type.id" :value="type.id">
          {{ type.typeName }}
        </option>
      </select>

      <div class="flex justify-end">
        <div >
              <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="48" height="48" viewBox="0 0 48 48">
                <path fill="#169154" d="M29,6H15.744C14.781,6,14,6.781,14,7.744v7.259h15V6z"></path>
                <path fill="#18482a" d="M14,33.054v7.202C14,41.219,14.781,42,15.743,42H29v-8.946H14z"></path>
                <path fill="#0c8045" d="M14 15.003H29V24.005000000000003H14z"></path>
                <path fill="#17472a" d="M14 24.005H29V33.055H14z"></path>
                <g>
                  <path fill="#29c27f" d="M42.256,6H29v9.003h15V7.744C44,6.781,43.219,6,42.256,6z"></path>
                  <path fill="#27663f" d="M29,33.054V42h13.257C43.219,42,44,41.219,44,40.257v-7.202H29z"></path>
                  <path fill="#19ac65" d="M29 15.003H44V24.005000000000003H29z"></path>
                  <path fill="#129652" d="M29 24.005H44V33.055H29z"></path>
                </g>
                <path fill="#0c7238"
                  d="M22.319,34H5.681C4.753,34,4,33.247,4,32.319V15.681C4,14.753,4.753,14,5.681,14h16.638 C23.247,14,24,14.753,24,15.681v16.638C24,33.247,23.247,34,22.319,34z">
                </path>
                <path fill="#fff"
                  d="M9.807 19L12.193 19 14.129 22.754 16.175 19 18.404 19 15.333 24 18.474 29 16.123 29 14.013 25.07 11.912 29 9.526 29 12.719 23.982z">
                </path>
              </svg>
            </div>
      </div>
    </div>

    <table class="min-w-full border-collapse border border-gray-200">
      <thead>
        <tr>
          <th class="border border-gray-200 px-4 py-2">#</th>
          <th class="border border-gray-200 px-4 py-2">Product Name</th>
          <th class="border border-gray-200 px-4 py-2">Stock</th>
          <th class="border border-gray-200 px-4 py-2">Type</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="products.length === 0">
          <td colspan="4" class="text-center">No products found.</td>
        </tr>
        <tr
          v-else
          v-for="product in products"
          :key="product.productId"
          @click="goToProductDetail(product.productId)"
          class="hover:bg-slate-400">
          <td class="border border-gray-200 px-4 py-2">
            <img
              :src="
                product.productPicture
                  ? 'http://erpstock.servehttp.com:9090/images/' +
                    product.productPicture
                  : 'https://lh3.googleusercontent.com/proxy/vfrcI3Ho8V8lLS1FWlXFKUAc9p85CQm9WxsUFwOm1zrLrYsStycX5NSOBJS4TYEEX5_3mQkd8QuqnIk'
              "
              :alt="
                product.productPicture ? 'Product Picture' : 'Default Picture'
              "
              class="w-16 h-16 object-cover" />
          </td>

          <td class="border border-gray-200 px-4 py-2">
            {{ product.productName }}
          </td>
          <td class="border border-gray-200 px-4 py-2">
            {{ product.productTotal }}
          </td>
          <td class="border border-gray-200 px-4 py-2">
            {{ getTypeName(product.productType) }}
          </td>
        </tr>
      </tbody>
    </table>

    <div class="mt-4 flex space-x-2">
      <button
        @click="changePage(currentPage - 1)"
        :disabled="currentPage === 0"
        class="px-4 py-2 bg-gray-300 text-black rounded">
        Previous
      </button>
      <button
        v-for="page in totalPages"
        :key="page"
        @click="changePage(page - 1)"
        class="px-4 py-2 bg-blue-500 text-white rounded">
        {{ page }}
      </button>
      <button
        @click="changePage(currentPage + 1)"
        :disabled="currentPage + 1 >= totalPages"
        class="px-4 py-2 bg-gray-300 text-black rounded">
        Next
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [],
      productType: [],
      currentPage: 0,
      totalPages: 1,
      isLoading: false,
      filters: {
        page: 0,
        perpage: 10,
        isuse: true,
        max: "desc",
        search: "",
        stock: null,
        productType: 0,
      },
    };
  },
  methods: {
    goToProductDetail(productId) {
      console.log("Product ID:", productId);
      this.$router.push(`/admin/${productId}`);
    },
    async fetchProducts() {
      // console.log("Current Filters:", this.filters);
      try {
        // console.log("Filters being sent to API:", this.filters);
        const response = await axios.post(
          "http://erpstock.servehttp.com:9090/api/products/products",
          this.filters
        );
        this.products = response.data.data;
      } catch (error) {
        console.error("Error fetching products:", error);
        this.products = [];
      }
    },
    async fetchProductTypes() {
      try {
        const response = await axios.get(
          "http://erpstock.servehttp.com:9090/api/products/get/producttype"
        );
        this.productType = response.data.data;
      } catch (error) {
        console.error(
          "Error fetching products:",
          error.response ? error.response.data : error.message
        );
        this.products = [];
      }
    },
    getTypeName(typeId) {
      const type = this.productType.find((item) => item.id === typeId);
      return type ? type.typeName : "Unknown Type";
    },
  },

  mounted() {
    this.fetchProducts();
    this.fetchProductTypes();
  },
};
</script>
