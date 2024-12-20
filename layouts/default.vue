<template>
  <header v-if="!isLoginPage" class="fixed top-0 left-0 w-full z-10 bg-white">
    <nav>
      <div class="grid grid-flow-col gap-4 p-4">
        <div class="flex justify-end">
          <span
            class="inline-block size-10 overflow-hidden rounded-full bg-white border border-black">
            <svg
              class="size-full text-black"
              fill="currentColor"
              viewBox="0 0 24 24">
              <path
                d="M24 20.993V24H0v-2.996A14.977 14.977 0 0112.004 15c4.904 0 9.26 2.354 11.996 5.993zM16.002 8.999a4 4 0 11-8 0 4 4 0 018 0z" />
            </svg>
          </span>
        </div>
      </div>
    </nav>
  </header>

  <div class="flex h-full">
    <aside
      v-if="!isLoginPage"
      :class="[
        isSidebarOpen ? 'translate-x-0' : '-translate-x-full',
        'fixed z-30 w-60 h-full bg-gray-800 text-white transition-transform xl:translate-x-0',
      ]"
      :aria-hidden="!isSidebarOpen">
      <div class="p-5 h-14">
        <button
          @click="toggleSidebar"
          class="text-xl focus:outline-none xl:hidden"
          aria-label="Toggle Sidebar">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6 text-white">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
          </svg>
        </button>
        <div class="text-xl flex justify-center text-white ml-10 md:block">
          Logo
        </div>
      </div>
      <nav class="p-4 h-5/6">
        <ul class="space-y-4">
          <li>
            <NuxtLink
              to="/admin/homepage"
              class="flex items-center p-2 rounded hover:bg-gray-700 hover:text-yellow-500">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="size-6">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
              </svg>
              <span class="ml-2">Home</span>
            </NuxtLink>
          </li>
          <li>
            <NuxtLink
              to="/admin/employee"
              class="flex items-center p-2 rounded hover:bg-gray-700 hover:text-yellow-500">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="size-6">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" />
              </svg>

              <span class="ml-2">Employee</span>
            </NuxtLink>
          </li>
          <li>
            <div
              @click="toggleDropdown"
              class="flex items-center p-2 rounded hover:bg-gray-700">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="size-6">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5M10 11.25h4M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z" />
              </svg>
              <button type="button" class="ml-2">Product</button>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="size-4 ml-12">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="m19.5 8.25-7.5 7.5-7.5-7.5" />
              </svg>
            </div>

            <ul v-if="isDropdownOpen" class="py-2 space-y-2">
              <li>
                <a
                  href="/admin/product"
                  class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                  Products
                </a>
              </li>
              <li>
                <a
                  href="/admin/addproduct"
                  class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                  Add Product
                </a>
              </li>
              <li>
                <a
                  href="#"
                  class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                  Invoice
                </a>
              </li>
            </ul>
          </li>
        </ul>
      </nav>

      <footer class="p-5">
        <NuxtLink to="/login" class="flex">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15m-3 0-3-3m0 0 3-3m-3 3H15" />
          </svg>
          <span class="ml-2">Logout</span>
        </NuxtLink>
      </footer>
    </aside>

    <main
      <div
      :class="[
        !isLoginPage ? 'block xl:ml-64 mt-20 mb-20' : 'block',
        !isLoginPage ? 'flex-1' : 'w-full',
      ]">
      <slot />
    </main>

    <footer
      v-if="!isLoginPage"
      class="fixed bottom-0 left-0 w-full bg-white shadow-lg z-10 flex items-center justify-between px-4 py-2 xl:hidden">
      <nuxt-link to="/admin/homepage">
        <div class="flex flex-col items-center text-gray-500">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
          </svg>

          <span class="text-xs">Home</span>
        </div>
      </nuxt-link>
      <nuxt-link to="/admin/product">
        <div class="flex flex-col items-center text-gray-500">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5M10 11.25h4M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z" />
          </svg>

          <span class="text-xs">Product</span>
        </div>
      </nuxt-link>

      <nuxt-link to="/admin/scan">
        <div class="relative flex justify-center">
          <button
            class="bg-black rounded-full w-16 h-16 flex items-center justify-center text-white absolute -top-10 shadow-md border-4 border-white">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 128 128"
              id="QrCode">
              <path
                stroke="#ffffff"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="5"
                d="M102 83L102 97C102 99.7614 99.7614 102 97 102L83 102M102 45L102 31C102 28.2386 99.7614 26 97 26L83 26M45 102L31 102C28.2386 102 26 99.7614 26 97L26 83M26 45L26 31C26 28.2386 28.2386 26 31 26L45 26"
                class="colorStroke000000 svgStroke"></path>
              <path
                stroke="#ffffff"
                stroke-linecap="round"
                stroke-width="5"
                d="M21 64H107"
                class="colorStroke000000 svgStroke"></path>
            </svg>
          </button>
        </div>
      </nuxt-link>

      <nuxt-link to="/admin/employee">
        <div class="flex flex-col items-center text-gray-500">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" />
          </svg>

          <span class="text-xs">Employee</span>
        </div>
      </nuxt-link>

      <nuxt-link to="/admin/Setting">
        <div class="flex flex-col items-center text-gray-500">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M9.594 3.94c.09-.542.56-.94 1.11-.94h2.593c.55 0 1.02.398 1.11.94l.213 1.281c.063.374.313.686.645.87.074.04.147.083.22.127.325.196.72.257 1.075.124l1.217-.456a1.125 1.125 0 0 1 1.37.49l1.296 2.247a1.125 1.125 0 0 1-.26 1.431l-1.003.827c-.293.241-.438.613-.43.992a7.723 7.723 0 0 1 0 .255c-.008.378.137.75.43.991l1.004.827c.424.35.534.955.26 1.43l-1.298 2.247a1.125 1.125 0 0 1-1.369.491l-1.217-.456c-.355-.133-.75-.072-1.076.124a6.47 6.47 0 0 1-.22.128c-.331.183-.581.495-.644.869l-.213 1.281c-.09.543-.56.94-1.11.94h-2.594c-.55 0-1.019-.398-1.11-.94l-.213-1.281c-.062-.374-.312-.686-.644-.87a6.52 6.52 0 0 1-.22-.127c-.325-.196-.72-.257-1.076-.124l-1.217.456a1.125 1.125 0 0 1-1.369-.49l-1.297-2.247a1.125 1.125 0 0 1 .26-1.431l1.004-.827c.292-.24.437-.613.43-.991a6.932 6.932 0 0 1 0-.255c.007-.38-.138-.751-.43-.992l-1.004-.827a1.125 1.125 0 0 1-.26-1.43l1.297-2.247a1.125 1.125 0 0 1 1.37-.491l1.216.456c.356.133.751.072 1.076-.124.072-.044.146-.086.22-.128.332-.183.582-.495.644-.869l.214-1.28Z" />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
          </svg>

          <span class="text-xs">Setting</span>
        </div>
      </nuxt-link>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDropdownOpen: false,
      isSidebarOpen: false, // State to control the sidebar visibility
    };
  },
  computed: {
    isLoginPage() {
      return this.$route.path === "/login";
    },
  },
  methods: {
    toggleSidebar() {
      this.isSidebarOpen = !this.isSidebarOpen;
      document.body.style.overflow = this.isSidebarOpen ? "hidden" : "auto"; // Prevent body scroll when sidebar is open
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
  },
};
</script>

<style></style>
