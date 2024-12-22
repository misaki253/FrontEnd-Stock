<template>
  <div class="min-h-screen bg-gray-100 flex flex-col items-center justify-center">
    <h1 class="text-2xl font-bold mb-6">สร้าง QR Code</h1>
    <div class="w-full max-w-md bg-white p-6 shadow rounded">
      <label for="text" class="block mb-2 font-medium">ข้อความ:</label>
      <input
        id="text"
        v-model="inputText"
        type="text"
        class="w-full border-gray-300 rounded p-2 mb-4"
        placeholder="กรอกข้อความ"
      />
      <button
        @click="generateQRCode"
        class="bg-blue-500 text-white px-4 py-2 rounded shadow"
      >
        สร้าง QR Code
      </button>
      <div v-if="qrCodeDataUrl" class="mt-6">
        <h2 class="text-lg font-semibold">QR Code:</h2>
        <img :src="qrCodeDataUrl" alt="QR Code" class="mt-2" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import * as QRCode from "qrcode";

const inputText = ref("");
const qrCodeDataUrl = ref(null);

const generateQRCode = async () => {
  if (inputText.value) {
    try {
      qrCodeDataUrl.value = await QRCode.toDataURL(inputText.value);
    } catch (error) {
      console.error("Error generating QR Code:", error);
    }
  }
};
</script>

<style>
/* สไตล์เพิ่มเติม */
</style>
