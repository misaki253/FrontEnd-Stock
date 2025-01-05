<template>
  <div class="p-4 bg-white rounded-lg shadow-lg">
    <h1 class="text-3xl font-semibold mb-4">รายการสินค้า</h1>
    <div class="md:flex justify-between">


      <div class="mb-4 flex space-x-4">
        <input v-model="filters.search" type="text" placeholder="ค้นหา" class="border p-2 rounded"
          @input="fetchProducts" />
        <select v-model="filters.productType" class="border p-2 rounded" @change="fetchProducts">
          <option value="">ทุกประเภท</option>
          <option v-for="type in productType" :key="type.id" :value="type.id">
            {{ type.typeName }}
          </option>
        </select>
      </div>


      <div class="hidden lg:flex justify-center space-x-4">
        <div @click="openModal">
          <button class="flex items-center space-x-2 bg-yellow-500 px-5 py-2 rounded">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
              stroke="currentColor" class="size-6">
              <path stroke-linecap="round" stroke-linejoin="round"
                d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m6.75 12-3-3m0 0-3 3m3-3v6m-1.5-15H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z" />
            </svg>

            <span>export</span>
          </button>
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

        <div>
          <button class="flex items-center space-x-2 bg-yellow-500 px-5 py-2 rounded">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
              stroke="currentColor" class="size-6">
              <path stroke-linecap="round" stroke-linejoin="round"
                d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5m8.25 3v6.75m0 0-3-3m3 3 3-3M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z" />
            </svg>

            <span>import</span>
            <input type="file" accept=".xlsx" @change="handleFileUpload" class="hidden" ref="fileInput" />
          </button>
        </div>
      </div>

    </div>

    <table class="min-w-full border-collapse border border-gray-200 mt-5 text-sm">
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


    <div v-if="showProductModal" class="fixed inset-0 bg-gray-500 bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white p-6 rounded shadow-lg w-96">
        <h2 class="text-lg font-bold mb-4">ข้อมูลสินค้า</h2>

        <!-- แสดงรายละเอียดสินค้า -->
        <div class="mb-4">
          <div class="flex justify-center">
            <img :src="selectedProduct.productPicture
              ? 'https://project-stock.onrender.com/images/' + selectedProduct.productPicture
              : 'https://icon-library.com/images/no-picture-available-icon/no-picture-available-icon-1.jpg'"
              alt="ภาพสินค้า" class="w-48 h-48 object-cover mb-4" />
          </div>
          <p><strong>ชื่อสินค้า:</strong> {{ selectedProduct.productName }}</p>
          <p><strong>รหัสสินค้า:</strong> {{ selectedProduct.productNo }}</p>
          <p><strong>สต็อก:</strong> {{ selectedProduct.productTotal }}</p>
          <p><strong>ประเภท:</strong> {{ getTypeName(selectedProduct.productType) }}</p>
        </div>

        <div class="flex justify-between mt-10">
          <div class="flex">
            <button @click="deleteProduct" class="bg-red-500 text-white px-4 py-2 rounded flex ">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round"
                  d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
              </svg>
              <span>ลบ</span>
            </button>
          </div>
          <button @click="closeProductModal" class="bg-gray-300 text-gray-700 px-4 py-2 rounded flex">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
              stroke="currentColor" class="size-6">
              <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
            </svg>

            <span>ปิด</span>
          </button>

        </div>
      </div>
    </div>


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

  </div>
</template>



<script>
import axios from "axios";

export default {
  layout: 'admin',
  data() {
    return {
      products: [],
      productType: [],
      currentPage: 0,
      totalPages: 0,
      isLoading: false,
      showModal: false,
      showProductModal: false,
      selectedProduct: null,
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
      const product = this.products.find(p => p.productNo === productId);
      if (product) {
        this.selectedProduct = product;
        this.showProductModal = true; // เปิด Modal
      }
    },

    // ปิด Modal
    closeProductModal() {
      this.showProductModal = false;
      this.selectedProduct = null; // รีเซ็ตข้อมูลสินค้า
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
    importProduct() {
      const fileInput = this.$refs.fileInput;
      if (fileInput) {
        fileInput.click();
      }
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

      const formData = new FormData();
      formData.append("file", file);

      axios.post("https://project-stock.onrender.com/api/products/import", formData, {
        headers: { "Content-Type": "multipart/form-data" },
      })
        .then(response => {
          alert("Product data imported successfully!");
          this.fetchProducts(); // Refresh the product list
        })
        .catch(error => {
          console.error("Error importing products:", error);
          alert("Failed to import products. Please try again.");
        });
    },



    async deleteProduct() {
      // ยืนยันการลบสินค้าจากชื่อสินค้า
      if (confirm(`คุณต้องการลบสินค้า ${this.selectedProduct.productName} หรือไม่?`)) {
        try {
          console.log("กำลังลบสินค้า:", this.selectedProduct);

          await this.updateProductStatus(this.selectedProduct.productId, false);

          this.closeProductModal();

          // รีเฟรชรายการสินค้า
          this.fetchProducts();
        } catch (error) {
          // การจัดการข้อผิดพลาด
          alert('ไม่สามารถลบสินค้าได้ กรุณาลองใหม่อีกครั้ง');
          console.error("เกิดข้อผิดพลาดในการลบสินค้า:", error);
        }
      }
    }
    ,

    async updateProductStatus(productId, isUse) {
      try {
        const response = await axios.post(
          `https://project-stock.onrender.com/api/products/edit/${productId}`,
          {
            isUse: isUse,
          }
        );

        if (response.status === 200) {
          console.log('สถานะสินค้าอัปเดตเรียบร้อย');
        } else {
          throw new Error(`การอัปเดตสถานะสินค้าไม่สำเร็จ: ${response.statusText}`);
        }
      } catch (error) {
        console.error('เกิดข้อผิดพลาดในการอัปเดตสถานะสินค้า:', error);
        throw error;
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
