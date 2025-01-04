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
                <tr v-for="stockout in productstock" :key="stockout.productId"
                  class="divide-x divide-gray-700 bg-zinc-800 text-white">
                  <td class="px-3 py-1.5 text-xs">{{ stockout.productNo }}</td>
                  <td class="px-3 truncate max-w-[200px] text-xs">
                    {{ stockout.productName }}
                  </td>
                  <td class="whitespace-pre px-3 text-xs">
                    {{ stockout.productType }}
                  </td>
                  <td class="px-3 text-xs">{{ stockout.productTotal }}</td>
                  <td class="px-3 text-xs"> {{ formatDate(stockout.createdDate) }}</td>
                  <td class="px-3 text-xs">{{ stockout.employee }}</td>
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
                <tr v-for="product in products" :key="product.productId"
                  class="divide-x divide-gray-700 bg-zinc-800 text-white">
                  <td class="px-3 py-1.5 text-xs">{{ product.productNo }}</td>
                  <td class="px-3 truncate max-w-[200px] text-xs">
                    {{ product.productName }}
                  </td>
                  <td class="whitespace-pre px-3 text-xs">
                    {{ product.productType }}
                  </td>
                  <td class="px-3 text-xs">{{ product.productTotal }}</td>
                  <td class="px-3 text-xs">{{ formatDate(product.createdDate, product.updatedDate) }}</td>
                  <td class="px-3 text-xs">{{ product.employee }}</td>
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
import { data } from 'autoprefixer';
import axios from 'axios';

export default {
  data() {
    return {
      productstock: [],
      products: [],
      filters: {
        page: 0,
        perpage: 10,
        isuse: true,
        max: "desc",
        search: "",
        productType: "",
      },
    }
  },

  methods: {
    async fetchProducts() {
      try {
        const response = await axios.post(
          "https://project-stock.onrender.com/api/products/products",
          {
            page: 0,
            perpage: 20,
            isuse: true,
            max: "desc",
            search: "",
            stock: null,
            productType: ""
          }
        );
        console.log("Products Response: ", response.data);
        const { data } = response.data; 
        if (data && data.length > 0) {
          this.products = data;
        } else {
          console.log("No products found");
        }
      } catch (error) {
        console.error("Error fetch product", error);
      }
    }

    ,

    async fetchProductstock() {
      try {
        const response = await axios.post(
          "https://project-stock.onrender.com/api/products/get/productstock", this.filters
        );


        const { data } = response.data;


        this.productstock = data;

      } catch (error) {
        console.error("Error fetching product stock:", error);
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
    }
    ,
  },

  mounted() {
    this.fetchProductstock();
    this.fetchProducts();
  },

};
</script>
