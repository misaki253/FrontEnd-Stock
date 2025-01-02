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
        <div v-if="scannedProducts.length > 0" class="mt-5">
          <table class="divide-y divide-gray-300 border-b">
            <thead>
              <tr>
                <th class="px-3 py-5 text-left">ชื่อสินค้า</th>
                <th class="px-3">จำนวน</th>
                <th class="px-3"></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(product, index) in scannedProducts" :key="index">
                <td class="w-full py-5">{{ product.productName }}</td>
                <td class="flex justify-center py-5">{{ product.productTotal }}</td>
                <td class="py-5">
                  <!-- ปุ่มจัดการเพิ่มเติม เช่น หักสินค้า -->
                </td>
              </tr>
            </tbody>
          </table>
        </div>
  
        <div v-else class="mt-5 text-center text-gray-500">
          ยังไม่มีสินค้าที่สแกน
        </div>
  
        <div class="mt-5">
          <nuxt-link to="/admin/scan" class="text-gray-400">+ เพิ่มสินค้า</nuxt-link>
        </div>
      </div>
  
      <footer class="fixed bottom-0 left-0 w-full bg-white text-black p-4 text-center">
        <button class="bg-yellow-500 px-5 py-2 rounded" @click="confirmAction">ยืนยัน</button>
      </footer>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  
  const scannedProducts = ref([]);  // เก็บข้อมูลสินค้าที่สแกนแล้ว
  const scannedProductNos = ref([]);  // เก็บข้อมูล productNo ที่สแกน
  
  // ฟังก์ชันดึงข้อมูลสินค้าที่สแกนแล้ว
  const fetchScannedProducts = async () => {
    try {
      // ดึงข้อมูลสินค้า (ปรับ URL ให้ตรงกับ API ที่ส่งข้อมูลสินค้า)
      const response = await axios.post("https://project-stock.onrender.com/api/products/products");
      console.log('API response:', response.data);  // ตรวจสอบข้อมูลที่ได้รับ
  
      const allProducts = response.data.data;
      console.log('All Products:', allProducts);  // ตรวจสอบข้อมูลสินค้า
  
      // ตรวจสอบว่า scannedProductNos มีข้อมูลหรือไม่
      console.log('Scanned Product Nos:', scannedProductNos.value);
  
      // กรองสินค้าที่มี productNo ตรงกับที่ถูกสแกน
      scannedProducts.value = allProducts.filter(product => 
        scannedProductNos.value.includes(product.productNo));  // กรองสินค้าที่มี productNo ตรง
  
      console.log('Scanned Products:', scannedProducts.value);  // ตรวจสอบสินค้าที่ถูกกรอง
  
    } catch (error) {
      console.error('Error fetching scanned products:', error);
    }
  };
  
  // ฟังก์ชันที่รับค่า productNo จากการสแกน
  const addScannedProductNo = (productNo) => {
    // ใช้ .value เพื่อเพิ่มข้อมูลเข้าไปใน scannedProductNos
    if (!scannedProductNos.value.includes(productNo)) {
      scannedProductNos.value.push(productNo);  // เพิ่ม productNo ที่สแกน
      console.log('Added scanned productNo:', productNo);  // ตรวจสอบค่าที่เพิ่มเข้าไป
    }
  };
  
  const confirmAction = () => {
    // ฟังก์ชันสำหรับการยืนยันการกระทำ
    console.log('ยืนยันการกระทำ');
  };
  
  onMounted(() => {
    fetchScannedProducts();  // เรียกใช้ฟังก์ชันหลังจาก component ถูกโหลด
  });
  
  // เมื่อกลับไปหน้าก่อนหน้า
  const goBack = () => {
    if (this.$router && this.$route) {
      if (window.history.length > 1) {
        this.$router.back();
      } else {
        this.$router.push("/");
      }
    } else {
      console.error("Vue Router ไม่พร้อมใช้งาน");
    }
  };
  </script>
  
  <style scoped>
  /* สไตล์เพิ่มเติม */
  </style>
  