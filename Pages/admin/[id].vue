<template>
  <div>
    <h1>Product Detail</h1>
    <p>Product ID: {{ productId }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      productId: this.$route.params.id,  // ดึงค่า id จาก URL
    };
  },
  async created() {
    // สามารถดึงข้อมูลสินค้าจาก API โดยใช้ this.productId
  }
};
</script>
