<template>
  <header v-if="!isLoginPage" class="fixed top-0 left-0 w-full z-10">
    <nav>
      <div class="bg-gray-800 h-14 flex items-center p-5">
        <button
          @click="toggleSidebar"
          class="text-xl focus:outline-none sm:hidden"
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
      </div>
    </nav>
  </header>

  <div class="flex h-full bg-gray-100">
    <aside
      v-if="!isLoginPage"
      :class="[
        isSidebarOpen ? 'translate-x-0' : '-translate-x-full',
        'fixed z-30 w-60 h-full bg-gray-800 text-white transition-transform sm:translate-x-0',
      ]"
      :aria-hidden="!isSidebarOpen">
      <div class="p-5 h-14">
        <button
          @click="toggleSidebar"
          class="text-xl focus:outline-none sm:hidden"
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
              to="/homepage"
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
                  d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
              </svg>
              <span class="ml-2">Home</span>
            </NuxtLink>
          </li>
          <li>
            <NuxtLink
              to="/employee"
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
                  d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" />
              </svg>

              <span class="ml-2">Employee</span>
            </NuxtLink>
          </li>
          <li>
            <NuxtLink
              to="/product"
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

              <span class="ml-2">Product</span>
            </NuxtLink>
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
      :class="isLoginPage ? 'w-full' : 'flex-1'"
      class="[ 'block md:ml-64 mt-20 , !isLoginPage ? 'flex-1' : 'w-full', ]">
      <slot />
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
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
  },
};
</script>

<style></style>
