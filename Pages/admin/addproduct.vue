<template>
  <div class="p-5 lg:py-5 lg:px-20">
    <div>
      <h1 class="text-3xl font-semibold">Add Product</h1>
    </div>
  <div class="bg-gray-200 p-10 mt-5 rounded-xl">
    <div class="flex flex-col-reverse sm:grid sm:grid-cols-2 gap-5">
      <div class="grid gap-y-5">
        <div>
          <label for="pname" class="block">ชื่อสินค้า</label>
          <input type="text" class="border rounded w-full p-1" required />
        </div>
        <div class="grid grid-cols-1 gap-5 sm:grid-cols-2">
          <div>
            <label for="pid" class="block">รหัสสินค้า</label>
            <input type="text" class="border rounded p-1 w-full" required />
          </div>
          <div>
            <label for="pid" class="block">แบรนด์</label>
            <input type="text" class="border rounded p-1 w-full" required />
          </div>
          <div>
            <label for="pid" class="block">ประเภทสินค้า</label>
            <input type="text" class="border rounded p-1 w-full" required />
          </div>
          <div>
            <label for="pid" class="block">จำนวน</label>
            <input type="text" class="border rounded p-1 w-full" required />
          </div>
        </div>
        <div>
          <label for="pname" class="block">คำอธิบายสินค้า</label>
          <textarea
            id="pname"
            class="break-words border rounded p-1 w-full h-32"
            required></textarea>
        </div>
      </div>

      <div class="mt-5 sm:mt-0 sm:block">
        <div class="flex flex-col items-center">
          <img
            src="https://media.istockphoto.com/id/1324356458/vector/picture-icon-photo-frame-symbol-landscape-sign-photograph-gallery-logo-web-interface-and.jpg?s=612x612&w=0&k=20&c=ZmXO4mSgNDPzDRX-F8OKCfmMqqHpqMV6jiNi00Ye7rE="
            class="w-40 sm:w-80" />
            <div>
            <input
              type="file"
              placeholder="You can't touch this"
              class="block file-input file-input-bordered w-full max-w-xs bg-white mt-5"
              Enabled /></div>
        </div>
      </div>
    </div>
    <div class="flex justify-center mt-20 gap-20 ">
      <button
        class="bg-gray-500 rounded px-5 py-1.5 text-white hover:bg-gray-800">
        Save
      </button>
      <button
        @click="navigateToPage"
        class="bg-gray-500 rounded px-5 py-1.5 text-white hover:bg-gray-800">
        Cancel
      </button>
    </div>
  </div>
</div>
</template>

<script>
export default {
  methods: {
    navigateToPage() {
      this.$router.push("/admin/product");
    },
  },
};
</script>

<style scoped></style>
