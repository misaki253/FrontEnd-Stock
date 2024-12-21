<template>
  <div>
    <div class="">
      <header class="fixed top-0 left-0 w-full z-10 bg-black">
        <nav>
          <div class="grid grid-flow-col gap-4 p-4">
            <div class="">
              <span class=" ">
                <svg
                  @click="goBack"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-6 text-white">
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M15.75 19.5 8.25 12l7.5-7.5" />
                </svg>
              </span>
            </div>
          </div>
        </nav>
      </header>
    </div>
    <div class="w-full h-screen bg-black relative">
      <!-- กล้องเต็มหน้าจอ -->
      <div id="scanner-container" class="absolute inset-0"></div>
      <!-- Modal for displaying scanned result -->
      <div
        v-if="isModalOpen"
        class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center">
        <div class="bg-white p-6 rounded-lg">
          <p class="text-xl font-bold text-black">Scanned Result:</p>
          <p class="text-lg text-black">{{ scannedResult }}</p>
          <button
            @click="closeModal"
            class="mt-4 bg-blue-500 text-white py-2 px-4 rounded">
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Quagga from "quagga";
  import jsQR from "jsqr";

  export default {
    data() {
      return {
        scannedResult: "",  // Ensure this is initialized properly
        qrCanvas: null,
        qrContext: null,
        isModalOpen: false, // New state to manage modal visibility
      };
    },
    mounted() {
      this.initScanner();
    },
    methods: {
  goBack() {
    this.$router.go(-1);
  },
  initScanner() {
    // Barcode Scanner
    Quagga.init(
      {
        inputStream: {
          type: "LiveStream",
          target: document.querySelector("#scanner-container"),
          constraints: {
            width: { ideal: 1280 }, // Set resolution
            height: { ideal: 720 },
            facingMode: "environment", // Use rear camera
          },
        },
        decoder: {
          readers: ["code_128_reader", "ean_reader"], // Supported barcode types
        },
      },
      (err) => {
        if (err) {
          console.error(err);
          return;
        }
        Quagga.start();
      }
    );

    Quagga.onDetected(this.onDetected);

    // QR Code Scanner
    this.initQrScanner();
  },
  initQrScanner() {
    const video = document.querySelector("video");
    const canvas = document.createElement("canvas");
    this.qrCanvas = canvas;
    this.qrContext = canvas.getContext("2d");

    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
      navigator.mediaDevices
        .getUserMedia({
          video: {
            facingMode: "environment", // Use rear camera
            width: { ideal: 1280 }, // Set resolution
            height: { ideal: 720 },
          },
        })
        .then((stream) => {
          video.srcObject = stream;
          video.play();
          this.scanQrCode(); // Start QR scanning
        });
    }
  },
  scanQrCode() {
    const video = document.querySelector("video");
    if (
      this.qrCanvas &&
      this.qrContext &&
      video.readyState === video.HAVE_ENOUGH_DATA
    ) {
      this.qrCanvas.width = video.videoWidth;
      this.qrCanvas.height = video.videoHeight;
      this.qrContext.drawImage(
        video,
        0,
        0,
        video.videoWidth,
        video.videoHeight
      );

      const imageData = this.qrContext.getImageData(
        0,
        0,
        video.videoWidth,
        video.videoHeight
      );
      const code = jsQR(imageData.data, imageData.width, imageData.height);

      if (code) {
        this.scannedResult = code.data;
        console.log("QR Code detected:", code.data);
        this.isModalOpen = true; // Open modal on QR code scan
      } else {
        requestAnimationFrame(this.scanQrCode);
      }
    } else {
      requestAnimationFrame(this.scanQrCode);
    }
  },
  onDetected(result) {
  if (result && result.codeResult && result.codeResult.code) {
    console.log("Barcode detected:", result.codeResult.code);
    this.scannedResult = result.codeResult.code;
    Quagga.stop(); // หยุดการสแกน
    this.isModalOpen = true; // เปิด modal เมื่อสแกนบาร์โค้ดสำเร็จ
  } else {
    console.error("Invalid result from Quagga:", result); // เพิ่มการแสดงข้อผิดพลาดหากไม่มีข้อมูล
  }
},

  closeModal() {
    this.isModalOpen = false;
    this.resetScanner(); // Reset scanner to continue scanning
  },
  resetScanner() {
    // Stop Quagga and restart it
    Quagga.stop();
    setTimeout(() => {
      Quagga.start();
    }, 100); // Add a small delay to ensure it starts cleanly

    // Restart QR code scanning
    const video = document.querySelector("video");
    if (video && video.srcObject) {
      const stream = video.srcObject;
      const tracks = stream.getTracks();
      tracks.forEach((track) => track.stop()); // Stop the video stream
    }

    // Reinitialize the QR scanner
    this.initQrScanner();

    // Reset scanned result
    this.scannedResult = ""; // Clear scanned result
  },
},

  };
</script>

<style>
#scanner-container {
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
video {
  width: 100%;
  height: 100%;
  object-fit: cover; /* ครอบคลุมพื้นที่หน้าจอ */
}
</style>
