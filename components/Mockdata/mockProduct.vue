<!-- mockProduct.vue -->
<script>
export default {
  data() {
    return {
      getMockData: [
        {
          p_id: 1,
          pname: "Product 1",
          p_type: "Type A",
          stock: 100,
          image: "path/to/image1.jpg",
        },
        {
          p_id: 2,
          pname: "Product 2",
          p_type: "Type B",
          stock: 50,
          image: "path/to/image2.jpg",
        },
        // More mock products
      ],
    };
  },
};
</script>
