<template>
  <div class="p-4 bg-white rounded-lg shadow-lg">
    <h1 class="text-3xl font-semibold mb-4">รายการสินค้า</h1>
    <div class="flex justify-between">
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
      <div class="">
        <div class="">
          <div @click="openModal" class="">

            <button class="">
              <svg class="w-6 h-6 text-gray-800 dark:text-black" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
                <path fill-rule="evenodd"
                  d="M9 7V2.221a2 2 0 0 0-.5.365L4.586 6.5a2 2 0 0 0-.365.5H9Zm2 0V2h7a2 2 0 0 1 2 2v9.293l-2-2a1 1 0 0 0-1.414 1.414l.293.293h-6.586a1 1 0 1 0 0 2h6.586l-.293.293A1 1 0 0 0 18 16.707l2-2V20a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V9h5a2 2 0 0 0 2-2Z"
                  clip-rule="evenodd" />
              </svg>

              export
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
        </div>

        <div>
          <svg class="w-6 h-6 text-gray-800 dark:text-black" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
            width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
            <path fill-rule="evenodd"
              d="M9 7V2.221a2 2 0 0 0-.5.365L4.586 6.5a2 2 0 0 0-.365.5H9Zm2 0V2h7a2 2 0 0 1 2 2v16a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2v-5h7.586l-.293.293a1 1 0 0 0 1.414 1.414l2-2a1 1 0 0 0 0-1.414l-2-2a1 1 0 0 0-1.414 1.414l.293.293H4V9h5a2 2 0 0 0 2-2Z"
              clip-rule="evenodd" />
          </svg>


          <input type="file" accept=".xlsx" @change="handleFileUpload" class="hidden" ref="fileInput" />

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


    <div v-if="showProductModal" class="fixed inset-0 bg-gray-500 bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white p-6 rounded shadow-lg w-96">
        <h2 class="text-lg font-bold mb-4">ข้อมูลสินค้า</h2>

        <!-- แสดงรายละเอียดสินค้า -->
        <div class="mb-4">
          <img
            :src="selectedProduct.productPicture
              ? 'https://project-stock.onrender.com/images/' + selectedProduct.productPicture
              : 'https://lh3.googleusercontent.com/proxy/vfrcI3Ho8V8lLS1FWlXFKUAc9p85CQm9WxsUFwOm1zrLrYsStycX5NSOBJS4TYEEX5_3mQkd8QuqnIk'"
            alt="ภาพสินค้า" class="w-16 h-16 object-cover mb-4" />
          <p><strong>ชื่อสินค้า:</strong> {{ selectedProduct.productName }}</p>
          <p><strong>รหัสสินค้า:</strong> {{ selectedProduct.productNo }}</p>
          <p><strong>สต็อก:</strong> {{ selectedProduct.productTotal }}</p>
          <p><strong>ประเภท:</strong> {{ getTypeName(selectedProduct.productType) }}</p>
        </div>

        <div class="flex justify-between mt-4">
          <button @click="editProduct" class="bg-blue-500 text-white px-4 py-2 rounded">
            แก้ไข
          </button>
          <button @click="deleteProduct" class="bg-red-500 text-white px-4 py-2 rounded">
            ลบ
          </button>
          <button @click="closeProductModal" class="bg-gray-300 text-gray-700 px-4 py-2 rounded">
            ปิด
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
      products: [],
      productType: [],
      currentPage: 0,
      totalPages: 0,
      isLoading: false,
      showModal: false,
      showProductModal: false, // เพิ่มตัวแปรสำหรับแสดง Modal
      selectedProduct: null, // เพิ่มตัวแปรสำหรับเก็บข้อมูลสินค้าที่เลือก
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
          // ลบสินค้าออกจากฐานข้อมูล
          await this.removeProductFromDatabase(this.selectedProduct.productNo);

          // หลังจากลบจากฐานข้อมูลสำเร็จ, ลบออกจาก products array
          this.products = this.products.filter(p => p.productNo !== this.selectedProduct.productNo);

          // ปิด Modal
          this.closeProductModal();
        } catch (error) {
          // การจัดการข้อผิดพลาด
          alert('ไม่สามารถลบสินค้าได้ กรุณาลองใหม่อีกครั้ง');
          console.error("เกิดข้อผิดพลาดในการลบสินค้า:", error);
        }
      }
    },

    // ฟังก์ชันสำหรับลบสินค้าออกจากฐานข้อมูล (ใช้ API)
    async removeProductFromDatabase(productNo) {
      try {
        const response = await axios.delete(`https://project-stock.onrender.com/api/products/${productNo}`);

        if (response.status === 200) {
          console.log('ลบสินค้าเรียบร้อย');
        } else {
          throw new Error(`การลบสินค้าไม่สำเร็จ: ${response.statusText}`);
        }
      } catch (error) {
        // การจัดการข้อผิดพลาดในการส่งคำขอ
        console.error('เกิดข้อผิดพลาดในการลบสินค้า:', error);
        throw error; // ข้ามข้อผิดพลาดไปยังฟังก์ชันที่เรียกใช้
      }
    }
    ,


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
