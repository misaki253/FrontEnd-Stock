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

    <div v-if="barcodeData" class="fixed bottom-0 left-0 w-full bg-green-500 text-white text-center p-4">
      <p>สแกนบาร์โค้ดสำเร็จ: {{ barcodeData }}</p>
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
        // ตัวอย่างการส่งข้อมูลสินค้าไปยัง API scanner
        const response = await axios.post('https://project-stock.onrender.com/api/products/products', {
          products: [
            {
              productId: barcode,  // ใส่รหัสสินค้าที่ได้จากการสแกน
              productTotal: 1,      // จำนวนที่ต้องการหัก
            }
          ]
        });

        // ถ้าสินค้าอัพเดตสำเร็จ
        console.log(response.data.message);
        this.$router.push("/admin/picking");  // ส่งไปที่หน้า /admin/picking
      } catch (error) {
        console.error('Error handling barcode:', error);
        // จัดการข้อผิดพลาดที่เกิดขึ้น
      }
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
