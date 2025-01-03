<template>
    <div>
        <div class="flex bg-black p-5 text-white">
            <svg @click="goBack" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6 ">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" />
            </svg>

            <h1 class="">รายการสินค้าที่สแกน</h1>
        </div>

        <div class="p-4 bg-white rounded-lg shadow-lg">
            <div class="mt-5">
                <table class="divide-y divide-gray-300 border-b">
                    <thead>
                        <tr>
                            <th class="px-3 py-5 text-left">ชื่อสินค้า</th>
                            <th class="px-3">จำนวน</th>
                            <th class="px-3"></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(product, index) in pickingList" :key="index">
                            <td class="w-full py-5">{{ product.productName }}</td>
                            <td class="flex justify-center py-5">{{ product.quantity }}</td>
                            <td class="py-5">
                                <button @click="removeProduct(index)" class="text-red-500">ลบ</button>
                            </td>
                        </tr>
                    </tbody>

                </table>
            </div>

            <div class="mt-5">
                <nuxt-link to="/admin/scan" class="text-gray-400">+ เพิ่มสินค้า</nuxt-link>
            </div>
        </div>

        <footer class="fixed bottom-0 left-0 w-full bg-white text-black p-4 text-center">
            <button @click="confirmDeduction" class="bg-yellow-500 px-5 py-2 rounded">ยืนยัน</button>
        </footer>
    </div>
</template>

<script setup>
definePageMeta({
    layout: false,
});
</script>

<script>
import axios from "axios";

export default {
  data() {
    return {
      pickingList: [], // เก็บรายการสินค้าที่สแกน
    };
  },
  methods: {
    goBack() {
      if (this.$router && this.$route) {
        if (window.history.length > 1) {
          this.$router.back();
        } else {
          this.$router.push("/");
        }
      } else {
        console.error("Vue Router ไม่พร้อมใช้งาน");
      }
    },
    loadPickingList() {
      const storedList = localStorage.getItem("pickingList");
      this.pickingList = storedList ? JSON.parse(storedList) : [];
    },
    removeProduct(index) {
      this.pickingList.splice(index, 1); // ลบสินค้าจากรายการ
      localStorage.setItem("pickingList", JSON.stringify(this.pickingList)); // อัปเดต localStorage
    },
    async confirmDeduction() {
      if (this.pickingList.length === 0) {
        alert("ไม่มีสินค้าในรายการ");
        return;
      }

      // เตรียมข้อมูลสำหรับส่ง API
      const payload = {
        products: this.pickingList.map((product) => ({
          productId: product.productNo, // ใช้ productNo เป็น productId
          productTotal: product.quantity, // จำนวนสินค้าที่หัก
        })),
      };

      try {
        const response = await axios.post(
          "http://erpstock.servehttp.com:9090/api/products/scanner",
          payload
        );

        if (response.status === 200) {
          alert("หักสินค้าเรียบร้อยแล้ว");
          localStorage.removeItem("pickingList"); // ล้างรายการใน localStorage
          this.pickingList = []; // เคลียร์รายการในหน้า
          this.$router.push("/admin/scan"); // กลับไปหน้าสแกนสินค้า
        } else {
          alert("เกิดข้อผิดพลาดในการหักสินค้า");
        }
      } catch (error) {
        console.error("เกิดข้อผิดพลาด:", error);
        alert("เกิดข้อผิดพลาดในการเชื่อมต่อกับเซิร์ฟเวอร์");
      }
    },
  },
  mounted() {
    this.loadPickingList(); // โหลดข้อมูลเมื่อเปิดหน้า
  },
};
</script>
