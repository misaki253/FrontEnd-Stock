<template>
    <div class="container mx-auto p-5">
      <h1 class="text-2xl font-bold">รายละเอียดสินค้า</h1>
      <p>รหัสสินค้า: {{ productId }}</p>
      <!-- แสดงข้อมูลอื่น -->
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        productId: null,
      };
    },
    created() {
      // ดึงรหัสสินค้าจาก URL
      this.productId = this.$route.params.p_id;
      // คุณสามารถใช้รหัสนี้ไปดึงข้อมูลเพิ่มเติมจาก API ได้
    },
  };
  </script>