<template>
  <div class="flex flex-col h-screen">
    <!-- Header -->
    <header class="flex bg-blue-500 text-white p-4">
      <svg @click="goBack" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
        stroke="currentColor" class="size-6 mr-5">
        <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" />
      </svg>

      <h1 class="text-lg font-bold">สแกน Barcode สินค้า</h1>
    </header>

    <div>
      <div class="flex justify-center bg-blue-300">
        <button class="p-5 bg-white">หักสินค้า</button>
        <button class="p-5">เช็คสินค้า</button>
      </div>
    </div>

    <div class="justify-center h-full relative">
      <section class="w-full h-full">
        <!-- แสดงกล้อง -->
        <video ref="scanner" class="w-full h-full object-cover"></video>
      </section>
    </div>

    <div v-if="foundProduct" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
      <div class="bg-white p-5 rounded-lg max-w-md w-full">
        <h3 class="text-xl font-bold mb-4">ข้อมูลสินค้า</h3>
        <p><strong>ชื่อสินค้า:</strong> {{ foundProduct.productName }}</p>
        <p><strong>รหัสสินค้า:</strong> {{ foundProduct.productNo }}</p>
        <p><strong>จำนวนคงเหลือ:</strong> {{ foundProduct.productTotal }}</p>
        <button @click="closeModal" class="mt-4 w-full bg-blue-500 text-white py-2 rounded-md">ปิด</button>
      </div>
    </div>
  </div>
</template>

<script>
import Quagga from "quagga"; // นำเข้า QuaggaJS
import axios from "axios"; // นำเข้า Axios สำหรับเรียก API

export default {
  data() {
    return {
      videoStream: null, // เก็บข้อมูลการสตรีมของกล้อง
      barcodeData: null,  // เก็บข้อมูลที่ได้จากบาร์โค้ด
      productInfo: null,
      foundProduct: null
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

    startCamera() {
      const constraints = {
        video: true, // เปิดกล้อง
      };
      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          this.videoStream = stream;
          const videoElement = this.$refs.scanner;
          videoElement.srcObject = stream; // ตั้งค่า stream ให้กับ video
          videoElement.play(); // เริ่มเล่น
          videoElement.classList.remove("hidden");

          // เริ่มการตรวจจับบาร์โค้ด
          this.scanBarcode(videoElement);
        })
        .catch((error) => {
          console.error("ไม่สามารถเปิดกล้องได้:", error);
        });
    },

    scanBarcode(videoElement) {
      Quagga.init(
        {
          inputStream: {
            type: "LiveStream",
            target: videoElement, // ตั้งค่าให้ Quagga ใช้ video element
          },
          decoder: {
            readers: ["code_128_reader", "ean_reader", "ean_8_reader", "upc_reader", "upc_e_reader"], // สามารถเพิ่มรูปแบบบาร์โค้ดที่ต้องการ
          },
        },
        (err) => {
          if (err) {
            console.error("ไม่สามารถเริ่ม Quagga ได้:", err);
            return;
          }
          Quagga.start(); // เริ่มการสแกน
        }
      );

      // ฟังก์ชันที่จะถูกเรียกเมื่อ Quagga สแกนบาร์โค้ดสำเร็จ
      Quagga.onDetected((data) => {
        if (data && data.codeResult && data.codeResult.code) {
          this.barcodeData = data.codeResult.code; // เก็บข้อมูลบาร์โค้ดที่ได้
          console.log("บาร์โค้ดที่สแกน:", data.codeResult.code);
          Quagga.stop(); // หยุดการสแกนหลังจากเจอครั้งแรก
          this.handleBarcode(data.codeResult.code); // ส่งข้อมูลบาร์โค้ดไปยังฟังก์ชันที่ต้องการ
        }
      });
    },

    async handleBarcode(barcode) {
      try {
        // ส่งคำขอ GET ไปยัง API เพื่อตรวจสอบสินค้าที่ตรงกับ barcode
        const response = await axios.post('https://project-stock.onrender.com/api/products/products', {
          params: { productNo: [barcode] }  // ส่ง barcode เป็น query parameter
        });

        if (response.status === 200 && response.data) {
          const products = response.data.data;  // ข้อมูลสินค้าในระบบ
          const product = products.find(p => p.productNo === barcode);  // ค้นหาสินค้าตาม barcode

          if (product) {
            console.log('พบสินค้าที่ตรงกับบาร์โค้ด:', product);
            // this.addProductToUpdate(product); // เพิ่มสินค้าลงใน array
            this.foundProduct = product;
          } else {
            console.error('ไม่พบสินค้าที่ตรงกับบาร์โค้ด');
          }
        } else {
          console.error('เกิดข้อผิดพลาดในการดึงข้อมูลสินค้า');
        }
      } catch (error) {
        console.error('เกิดข้อผิดพลาดในการตรวจสอบสินค้า:', error);
      }
    },
    // async proceedToDeduct() {
    //   if (this.productsToUpdate.length > 0) {
    //     try {
    //       const response = await axios.post('https://project-stock.onrender.com/api/products/scanner', {
    //         products: this.productsToUpdate, // ส่ง array ของสินค้า
    //       });

    //       if (response.status === 200) {
    //         console.log(response.data.message); // แสดงข้อความที่ได้รับจาก API

    //         // ส่งข้อมูลสินค้าที่หักไปยังหน้า /admin/picking
    //         this.$router.push({
    //           path: '/admin/picking',
    //           state: { products: this.productsToUpdate }
    //         });
    //       } else {
    //         console.error('ไม่สามารถอัปเดตสินค้า');
    //       }
    //     } catch (error) {
    //       console.error('เกิดข้อผิดพลาดในการอัปเดตสินค้า:', error);
    //     }
    //   } else {
    //     console.error("ไม่มีสินค้าที่จะหัก");
    //   }
    // },

    closeModal() {
      this.foundProduct = null; // ปิดโมเดล
    },

  },
  mounted() {
    this.startCamera();
  },
  beforeDestroy() {
    if (this.videoStream) {
      this.videoStream.getTracks().forEach((track) => track.stop());
    }
    Quagga.stop(); // หยุด Quagga เมื่อออกจากหน้านี้
  },
};
</script>
