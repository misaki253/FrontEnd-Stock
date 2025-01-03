<template>
  <div class="flex flex-col h-screen">
    <!-- Header -->
    <header class="flex bg-black text-white px-4 py-7">
      <svg @click="goBack" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
        stroke="currentColor" class="size-6 mr-5">
        <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" />
      </svg>

      <h1 class="text-lg font-bold">สแกน Barcode สินค้า</h1>
    </header>

    <div class=" ">
      <div class="bg-black text-white grid grid-cols-2 min-w-full ">
        <button @click="setScanMode('deduct')"
          :class="{ 'bg-yellow-500 text-black': scanMode === 'deduct', 'bg-transparent': scanMode !== 'deduct' }"
          class="py-2 text-center">
          หักสินค้า
        </button>
        <button @click="setScanMode('check')"
          :class="{ 'bg-yellow-500 text-black': scanMode === 'check', 'bg-transparent': scanMode !== 'check' }"
          class="text-center">
          เช็คสินค้า
        </button>
      </div>
    </div>

    <div class="justify-center h-full relative">
      <section class="w-full h-full">
        <!-- แสดงกล้อง -->
        <video ref="scanner" class="w-full h-full object-cover"></video>
      </section>
    </div>

    <div v-if="foundProduct && scanMode === 'check'"
      class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
      <div class="bg-white p-5 rounded-lg max-w-md w-full">
        <h3 class="text-xl font-bold mb-4">ข้อมูลสินค้า</h3>
        <img :src="'http://erpstock.servehttp.com:9090/images/' + foundProduct.productPicture" alt="Product Image"
          class="w-full h-auto rounded-md mb-4">

        <p><strong>ชื่อสินค้า:</strong> {{ foundProduct.productName }}</p>
        <p><strong>จำนวนคงเหลือ:</strong> {{ foundProduct.productTotal }}</p>
        <button @click="closeModal" class="mt-4 w-full bg-blue-500 text-white py-2 rounded-md">ปิด</button>
      </div>
    </div>

    <div class="flex justify-center items-center h-10 bg-black text-white">
      วาง Barcode ให้อยู่ในตำแหน่ง
    </div>
  </div>
</template>

<script>
import Quagga from "quagga"; // นำเข้า QuaggaJS
import axios from "axios"; // นำเข้า Axios สำหรับเรียก API
import jsQR from "jsqr";
export default {
  data() {
    return {
      videoStream: null,
      barcodeData: null,
      productInfo: null,
      foundProduct: null,
      scanMode: 'deduct',
      pickingList: [],
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

    setScanMode(mode) {
      this.scanMode = mode; // กำหนดโหมดการสแกน
      this.startCamera(); // เริ่มการสแกนใหม่ทุกครั้งที่เปลี่ยนโหมด
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
      // Initialize Quagga for barcode scanning
      Quagga.init(
        {
          inputStream: {
            type: "LiveStream",
            target: videoElement,
          },
          decoder: {
            readers: ["code_128_reader", "ean_reader", "ean_8_reader", "upc_reader", "upc_e_reader"],
          },
        },
        (err) => {
          if (err) {
            console.error("Unable to initialize Quagga:", err);
            return;
          }
          Quagga.start();
        }
      );


      Quagga.onDetected((data) => {
        if (data && data.codeResult && data.codeResult.code) {
          this.barcodeData = data.codeResult.code;
          console.log("Scanned barcode:", data.codeResult.code);
          Quagga.stop();
          this.handleBarcode(data.codeResult.code);
        }
      });


      const canvasElement = document.createElement("canvas");
      const canvasContext = canvasElement.getContext("2d");

      const scanQRCode = () => {
        const videoWidth = videoElement.videoWidth;
        const videoHeight = videoElement.videoHeight;


        if (videoWidth === 0 || videoHeight === 0) {
          return requestAnimationFrame(scanQRCode);
        }

        canvasElement.width = videoWidth;
        canvasElement.height = videoHeight;
        canvasContext.drawImage(videoElement, 0, 0, videoWidth, videoHeight);

        const imageData = canvasContext.getImageData(0, 0, videoWidth, videoHeight);
        const code = jsQR(imageData.data, videoWidth, videoHeight);

        if (code) {
          console.log("Scanned QR code:", code.data);
          this.handleBarcode(code.data);
        }

        requestAnimationFrame(scanQRCode); // Continue scanning for QR codes
      };

      scanQRCode();
    },

    async handleBarcode(barcode) {
      try {
        const response = await axios.post('http://erpstock.servehttp.com:9090/api/products/products', {
          search: barcode,
        });

        if (response.status === 200 && response.data) {
          const products = response.data.data;
          const product = products.find((p) => p.productNo === barcode);

          if (product) {
            product.productPicture = product.productPicture || 'default-image.jpg';

            if (this.scanMode === 'deduct') {
              this.handleDeduct(product);
            } else if (this.scanMode === 'check') {
              this.foundProduct = product;
            }
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

    handleDeduct(product) {
      const storedPickingList = JSON.parse(localStorage.getItem('pickingList')) || [];
      const existingProduct = storedPickingList.find((item) => item.productNo === product.productNo);

      if (existingProduct) {
        existingProduct.quantity += 1;
      } else {
        storedPickingList.push({ ...product, quantity: 1 });
      }

      localStorage.setItem('pickingList', JSON.stringify(storedPickingList));

      console.log('อัปเดตข้อมูลสินค้าใน localStorage:', storedPickingList);

      this.$router.push('/admin/picking');
    },


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
