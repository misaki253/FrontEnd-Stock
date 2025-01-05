<template>
  <div class="p-4 bg-white rounded-lg shadow-lg">
    <input v-model="filters.search" type="text" placeholder="ค้นหา" class="border p-2 rounded"
          @input="fetchProducts" />
    <!-- Search Input -->
    <!-- <div class="mb-4 relative">
      <label for="search" class="block text-sm font-medium text-gray-700">
        ค้นหาสินค้า
      </label>
      <input
        type="text"
        id="search"
        v-model="searchQuery"
        placeholder="ค้นหาสินค้า"
        class="mt-2 p-2 w-full border border-gray-300 rounded-md"
        @input="fetchProducts" />
      
      <div
        v-if="searchQuery && filteredProducts.length > 0"
        class="mt-2 absolute w-full bg-white border border-gray-300 rounded-md shadow-lg z-10">
        <ul>
          <li
            v-for="(product, index) in filteredProducts"
            :key="index"
            class="p-2 cursor-pointer hover:bg-gray-100 flex items-center space-x-2"
            @click="selectProduct(product)">
            <img
              :src="product.imageUrl"
              alt="Product Image"
              class="w-8 h-8 object-cover rounded-full" />
            <span>{{ product.productName }}</span>
          </li>
        </ul>
      </div>
      
      <div
        v-else-if="searchQuery"
        class="mt-2 absolute w-full bg-white border border-gray-300 rounded-md shadow-lg">
        <p class="p-2">ไม่พบสินค้าที่ค้นหา</p>
      </div>
    </div> -->
    <table class="min-w-full border-collapse border border-gray-200 mt-5 text-sm">
      <thead>
        <tr>
          <th class="border border-gray-200 px-4 py-2 w-10 text-center">
            <input
              type="checkbox"
              v-model="selectAll"
              @change="toggleSelectAll"
            />
          </th>
          <th class="border border-gray-200 px-4 py-2">ลำดับ</th>
          <th class="border border-gray-200 px-4 py-2">ชื่อสินค้า</th>
          <th class="border border-gray-200 px-4 py-2">สต็อก</th>
          <th class="border border-gray-200 px-4 py-2">ประเภท</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="products.length === 0">
          <td colspan="5" class="text-center">ไม่พบข้อมูลสินค้า</td>
        </tr>
        <tr v-else v-for="(product, index) in products" :key="product.productNo"
          @click="goToProductDetail(product.productNo)" class="hover:bg-slate-400">
          <td class="border border-gray-200 px-4 py-2 w-10 text-center">
            <input
              type="checkbox"
              :value="product"
              v-model="selectedProducts"
            />
          </td>
          <td class="border border-gray-200 px-4 py-2 w-10 text-center">{{ startIndex + index + 1 }}</td>
          <td class="flex items-center border border-gray-200 px-4 py-2">
            <img :src="product.productPicture
              ? 'https://project-stock.onrender.com/images/' + product.productPicture
              : 'https://icon-library.com/images/no-picture-available-icon/no-picture-available-icon-1.jpg'"
              :alt="product.productPicture ? 'ภาพสินค้า' : 'ภาพเริ่มต้น'" class="w-10 h-10 object-cover" />

            <span class="ml-5 truncate">{{ product.productName }}</span>
          </td>
          <td class="border border-gray-200 px-4 py-2 w-20 text-center">{{ product.productTotal }}</td>
          <td class="border border-gray-200 px-4 py-2 w-36 text-center">{{ getTypeName(product.productType) }}</td>

        </tr>
      </tbody>


    </table>
    <!-- <table
      v-if="selectedProducts.length > 0"
      class="min-w-full bg-white border border-gray-300 rounded-lg shadow-md mb-4">
      <thead>
        <tr class="bg-gray-100">
          <th class="p-2 text-left text-sm font-semibold text-gray-600">
            Product
          </th>
          <th class="p-2 text-left text-sm font-semibold text-gray-600">Qty</th>
          <th class="p-2 text-left text-sm font-semibold text-gray-600">
            Action
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(product, index) in selectedProducts"
          :key="product.productId"
          class="border-b">
          <td class="p-2">
            <img
              :src="product.imageUrl"
              :alt="product.productName"
              class="w-16 h-16 object-cover" />
            {{ product.productName }}
          </td>
          <td class="p-2">
            <input
              type="number"
              v-model="product.productTotal"
              min="1"
              class="w-16 text-center p-1 border border-gray-300 rounded-md" />
          </td>
          <td class="p-2 text-center">
            <button @click="removeProduct(index)" class="text-red-500">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table> -->
    <div class="md:flex justify-between mt-5">
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

      <div class="flex justify-center md: items-center justify-end space-x-2">
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
  <div class="border-t mt-5 p-5 flex  justify-between">
    <div class="flex">
      <label for="codeType" class="flex items-center mr-5 font-medium text-gray-700">
        เลือกรูปแบบ
      </label>
      <select
        id="codeType"
        v-model="isQr"
        class="p-2 border border-gray-300 rounded-md">
        <option :value="true">QR Code</option>
        <option :value="false">Barcode</option>
      </select>
    </div>

    <div class="">
      <button
        @click="generateBarcodes"
        class="bg-orange-500 text-white p-2 rounded-md hover:bg-orange-600 mr-5">
        สร้างโค้ด
      </button>
      <button
        @click="resetBarcodes"
        class="bg-gray-300 text-gray-700 p-2 rounded-md hover:bg-gray-400">
        รีเซ็ต
      </button>
    </div>

    <div v-if="generatedBarcodes.length > 0">
      <div v-for="barcode in generatedBarcodes" :key="barcode">
        <img
          :src="'data:image/png;base64,' + barcode"
          alt="Generated Code" />
      </div>
    </div>
  </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data() {
    return {
      searchQuery: "",
      isQr: true, // Default to QR Code
      products: [],
      productType: [],
      currentPage: 0,
      totalPages: 0,
      selectAll: false,
      selectedProducts: [],
      generatedBarcodes: [],
      showBarcodePopup: false,
      filters: {
        page: 0,
        perpage: 5,
        isuse: true,
        max: "desc",
        search: "",
        stock: null,
        productType: 0,
      },
    };
  },
  computed: {
    filteredProducts() {
      return this.products.filter((product) => {
        const query = this.searchQuery.toLowerCase();
        return (
          product.productName.toLowerCase().includes(query) ||
          product.productNo.toLowerCase().includes(query)
        );
      });
    },
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
    toggleSelectAll() {
      if (this.selectAll) {
        this.selectedProducts = [...this.products];
      } else {
        this.selectedProducts = [];
      }
    },
    async fetchProducts() {
      try {

        this.products = [];

        const response = await axios.post(
          "https://project-stock.onrender.com/api/products/products",
          this.filters
        );

        const { data, totalCount } = response.data;

        const uniqueProducts = data.reduce((acc, product) => {
          const existingProduct = acc.find(item => item.productNo === product.productNo);

          if (existingProduct) {
            existingProduct.productTotal += product.productTotal;
          } else {
            acc.push(product);
          }
          return acc;
        }, []);

        this.products = uniqueProducts;
        this.totalCount = totalCount;
        this.totalPages = Math.ceil(this.totalCount / this.filters.perpage);

        if (isNaN(this.totalPages) || this.totalPages <= 0) {
          this.totalPages = 1;
        }
      } catch (error) {
        console.error("Error fetching products:", error);
        this.products = [];
        this.totalCount = 0;
        this.totalPages = 1;
      }
    },
    selectProduct(product) {
      if (
        !this.selectedProducts.some((p) => p.productId === product.productId)
      ) {
        this.selectedProducts.push({ ...product, productTotal: 1 });
        this.searchQuery = "";
      }
    },
    removeProduct(index) {
      this.selectedProducts.splice(index, 1);
    },

    async generateBarcodes() {
      if (this.selectedProducts.length === 0) {
        alert("กรุณาเลือกสินค้าก่อนเพื่อสร้างโค้ด.");
        return;
      }

      const productsToGenerate = this.selectedProducts.map((product) => ({
        productId: product.productId,
        productTotal: product.productTotal,
      }));

      try {
        const response = await axios.post(
          "https://project-stock.onrender.com/api/products/genbarcode",
          {
            products: productsToGenerate,
            isQr: this.isQr, // Dynamically set based on selection
          },
          { responseType: "arraybuffer" }
        );

        const blob = new Blob([response.data], { type: "application/pdf" });
        const url = window.URL.createObjectURL(blob);
        const link = document.createElement("a");
        link.href = url;
        link.download = this.isQr ? "qrcodes.pdf" : "barcodes.pdf";
        link.click();

        this.generatedBarcodes = [];
      } catch (error) {
        console.error("Error generating codes:", error);
        alert("เกิดข้อผิดพลาดในการสร้างโค้ด. กรุณาลองใหม่.");
      }
    },

    resetBarcodes() {
      this.selectedProducts = [];
    },
    changePage(page) {
      if (page >= 0 && page < this.totalPages) {
        this.currentPage = page;
        this.filters.page = page;
        this.fetchProducts();
      }
    },
    async fetchProductTypes() {
      try {
        const response = await axios.get(
          "https://project-stock.onrender.com/api/products/get/producttype"
        );
        this.productType = response.data.data;
      } catch (error) {
        console.error("Error fetching product types:", error);
        this.productType = [];
      }
    },
    
    getTypeName(typeId) {
      const type = this.productType.find(item => item.id === typeId);
      return type ? type.typeName : "Unknown Type";
    },
  },
  mounted() {
    this.fetchProducts();
    this.fetchProductTypes();
  },
};
</script>
