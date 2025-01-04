<template>
  <div class="p-4 bg-white rounded-lg shadow-lg">
    <h1 class="text-3xl font-semibold mb-4">รายการสินค้า</h1>
    <div class="mb-4 flex space-x-4">
      <input v-model="filters.search" type="text" placeholder="ค้นหา" class="border p-2 rounded"
        @input="fetchProducts" />
      <select v-model="filters.productType" class="border p-2 rounded" @change="fetchProducts">
        <option value="">ทุกประเภท</option>
        <option v-for="type in productType" :key="type.id" :value="type.id">
          {{ type.typeName }}
        </option>
      </select>

      <div class="flex justify-end">
        <div class="p-4">
          <div @click="openModal">
            <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 48 48">
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

          <!-- Modal -->
          <div v-if="showModal" class="fixed inset-0 bg-gray-500 bg-opacity-50 flex items-center justify-center z-50">
            <div class="bg-white p-6 rounded shadow-lg w-96">
              <h2 class="text-lg font-bold mb-4">เลือกช่วงวันที่</h2>

              <label class="block mb-2 text-sm font-medium text-gray-700">วันที่เริ่มต้น:</label>
              <input type="date" v-model="dataFrom" class="border border-gray-300 rounded w-full p-2 mb-4" />

              <label class="block mb-2 text-sm font-medium text-gray-700">วันที่สิ้นสุด:</label>
              <input type="date" v-model="dataTo" class="border border-gray-300 rounded w-full p-2 mb-4" />

              <div class="flex justify-end">
                <button @click="closeModal" class="bg-gray-300 text-gray-700 px-4 py-2 rounded mr-2">
                  ยกเลิก
                </button>
                <button @click="exportData" class="bg-blue-500 text-white px-4 py-2 rounded">
                  ส่งออก
                </button>
              </div>
            </div>
          </div>
        </div>

        <div>
          <svg @click="importProduct" xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 6.35 6.35">
            <path
              d="M4.497 0a.265.265 0 0 0-.264.263v1.06c0 .435.358.793.793.793h1.06a.265.265 0 0 0 0-.529h-1.06a.26.26 0 0 1-.263-.264V.263A.265.265 0 0 0 4.497 0Z"
              color="#000"></path>
            <path
              d="M2.117 0a.798.798 0 0 0-.795.793V2.91a.265.265 0 0 0 .266.266.265.265 0 0 0 .264-.266V.793a.26.26 0 0 1 .265-.264H4.39L5.82 1.961v3.596a.26.26 0 0 1-.263.263h-3.44a.26.26 0 0 1-.265-.263v-.53a.265.265 0 0 0-.264-.265.265.265 0 0 0-.266.265v.53c0 .435.36.793.795.793h3.44a.796.796 0 0 0 .793-.793V1.852a.265.265 0 0 0-.077-.188L4.686.078A.265.265 0 0 0 4.498 0Z"
              color="#000"></path>
            <path d="M.264 4.234a.265.265 0 0 1 0-.529h3.175a.265.265 0 1 1 0 .53z" color="#000"></path>
            <path
              d="M2.723 2.988a.265.265 0 0 0 0 .373l.607.608-.607.607a.265.265 0 0 0 0 .373.265.265 0 0 0 .375 0l.793-.793a.265.265 0 0 0 0-.375l-.793-.793a.265.265 0 0 0-.375 0z"
              color="#000" style="-inkscape-stroke:none"></path>
          </svg>
          <input type="file" @change="handleFileUpload" class="hidden" ref="fileInput" />
        </div>
      </div>
    </div>

    <table class="min-w-full border-collapse border border-gray-200">
      <thead>
        <tr>
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
          <td class="border border-gray-200 px-4 py-2">{{ startIndex + index + 1 }}</td>
          <td class="border border-gray-200 px-4 py-2">
            <img
              :src="product.productPicture
                ? 'https://project-stock.onrender.com/images/' + product.productPicture
                : 'https://lh3.googleusercontent.com/proxy/vfrcI3Ho8V8lLS1FWlXFKUAc9p85CQm9WxsUFwOm1zrLrYsStycX5NSOBJS4TYEEX5_3mQkd8QuqnIk'"
              :alt="product.productPicture ? 'ภาพสินค้า' : 'ภาพเริ่มต้น'" class="w-16 h-16 object-cover" />

            {{ product.productName }} {{ product.productNo }}
          </td>
          <td class="border border-gray-200 px-4 py-2">{{ product.productTotal }}</td>
          <td class="border border-gray-200 px-4 py-2">{{ getTypeName(product.productType) }}</td>

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
      products: [],
      productType: [],
      currentPage: 0,
      totalPages: 0,
      isLoading: false,
      showModal: false,
      dataFrom: null,
      dataTo: null,
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
    goToProductDetail(productId) {
      console.log("Product ID:", productId);
      this.$router.push(`/admin/${productId}`);
    },

    async fetchProducts() {
      try {
        // เคลียร์ข้อมูลสินค้าเดิม
        this.products = [];

        const response = await axios.post(
          "https://project-stock.onrender.com/api/products/products",
          this.filters
        );

        const { data, totalCount } = response.data;

        // กรองสินค้าที่มี productNo ซ้ำกันให้รวมกัน
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

    async exportData() {
      if (!this.dataFrom || !this.dataTo) {
        alert("กรุณาเลือกวันที่เริ่มต้นและวันที่สิ้นสุด");
        return;
      }

      try {
        const response = await axios.post(
          "https://project-stock.onrender.com/api/products/export",
          {
            datafrom: this.dataFrom,
            datato: this.dataTo,
          },
          {
            responseType: "blob",
          }
        );

        const url = window.URL.createObjectURL(new Blob([response.data]));
        const link = document.createElement("a");
        link.href = url;
        link.setAttribute("download", `exported_data_${this.dataFrom}_${this.dataTo}.xlsx`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);

        this.closeModal();
      } catch (error) {
        console.error("Error exporting data:", error);
        alert("การส่งออกข้อมูลล้มเหลว กรุณาลองใหม่");
      }
    },


    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },

    changePage(page) {
      if (page >= 0 && page < this.totalPages) {
        this.currentPage = page;
        this.filters.page = page;
        this.fetchProducts();
      }
    },
  },

  mounted() {
    this.fetchProducts();
    this.fetchProductTypes();
  },
};
</script>
