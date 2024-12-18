<template>
  <header v-if="!isLoginPage" class="fixed top-0 left-0 w-full z-10 bg-white">
    <nav>
      <div class="grid grid-flow-col gap-4 p-4">
        <div class="">
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
              class="size-6 text-gray-800">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
            </svg>
          </button>
        </div>
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

  <div class="flex h-full ">
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
            <div @click="toggleDropdown" class="flex items-center p-2 rounded hover:bg-gray-700">
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
              <button type="button" class="ml-2">
                Product
              </button>
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
                  href="#"
                  class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                  Billing
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
        !isLoginPage ? 'block ml-5 xl:ml-64 mt-20' : 'block',
        !isLoginPage ? 'flex-1' : 'w-full',
      ]">
      <slot />
    </main>
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
