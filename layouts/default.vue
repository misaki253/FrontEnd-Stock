<template>
    <header v-if="!isScanPage" class="fixed top-0 left-0 w-full z-10 bg-white">
        <nav>
            <div v-if="userRole === 'admin'" class="grid grid-flow-col gap-4 p-4">
                <div class="flex justify-end">
                    <div @click="openModal">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" transform="rotate(180)"
                            stroke-width="1.5" stroke="currentColor" class="size-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15m-3 0-3-3m0 0 3-3m-3 3H15" />
                        </svg>

                    </div>
                </div>
            </div>
        </nav>
    </header>

    <div class="flex h-full">
        <aside v-if="!isScanPage"
            :class="[isSidebarOpen ? 'translate-x-0' : '-translate-x-full', 'fixed z-30 w-60 h-full bg-gray-800 text-white transition-transform xl:translate-x-0 flex flex-col']">
            <div class="p-5 h-14">
                <button @click="toggleSidebar" class="text-xl focus:outline-none xl:hidden" aria-label="Toggle Sidebar">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="size-6 text-white">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
                    </svg>
                </button>
                <div class="text-xl flex justify-center text-white ml-10 md:block">
                    Logo
                </div>
            </div>
            <nav class="p-4 h-full flex-1">
                <ul class="space-y-4">
                    <li>
                        <NuxtLink :to="userRole === 'employee' ? '/employee/homepage' : '/admin/homepage'"
                            class="flex items-center p-2 rounded hover:bg-gray-700 hover:text-yellow-500">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                                stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round"
                                    d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
                            </svg>
                            <span class="ml-2">Home</span>
                        </NuxtLink>
                    </li>
                    <li v-if="userRole === 'admin'">
                        <NuxtLink to="/admin/employee"
                            class="flex items-center p-2 rounded hover:bg-gray-700 hover:text-yellow-500">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                                stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round"
                                    d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" />
                            </svg>
                            <span class="ml-2">Employee</span>
                        </NuxtLink>
                    </li>
                    <li>
                        <div @click="toggleDropdown" class="flex items-center p-2 rounded hover:bg-gray-700">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                                stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round"
                                    d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5M10 11.25h4M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z" />
                            </svg>
                            <button type="button" class="ml-2">Product</button>
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                                stroke="currentColor" class="size-4 ml-12">
                                <path stroke-linecap="round" stroke-linejoin="round" d="m19.5 8.25-7.5 7.5-7.5-7.5" />
                            </svg>
                        </div>
                        <ul v-if="isDropdownOpen" class="py-2 space-y-2">
                            <li>
                                <a :href="userRole === 'employee' ? '/employee/product' : '/admin/product'"
                                    class="flex items-center w-full p-2 text-gray-900 tproductransition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                                    Products
                                </a>
                            </li>
                            <li>
                                <a v-if="userRole === 'admin'" href="/admin/addproduct"
                                    class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                                    Add Product
                                </a>
                            </li>
                            <li>
                                <a href="/genbarcode"
                                    class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700 hover:text-yellow-500">
                                    Print Barcode/QR
                                </a>
                            </li>
                        </ul>
                    </li>
                </ul>
            </nav>

            <!-- Footer -->
            <footer class="p-5">
                <div @click="openModal" class="flex items-center">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15m-3 0-3-3m0 0 3-3m-3 3H15" />
                    </svg>
                    <span>Logout</span>
                </div>
            </footer>
        </aside>


        <main :class="[
            !isScanPage ? 'block xl:ml-64 mt-20 mb-20' : 'block',
            !isScanPage ? 'flex-1' : 'w-full',
        ]">
            <slot />
        </main>

        <footer v-if="!isScanPage"
            class="fixed bottom-0 left-0 w-full bg-white shadow-lg z-10 flex items-center justify-between px-4 py-2 xl:hidden">
            <nuxt-link :to="userRole === 'employee' ? '/employee/homepage' : '/admin/homepage'">
                <div class="flex flex-col items-center text-gray-500">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
                    </svg>

                    <span class="text-xs">Home</span>
                </div>
            </nuxt-link>
            <nuxt-link :to="userRole === 'employee' ? '/employee/product' : '/admin/product'">
                <div class="flex flex-col items-center text-gray-500">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5M10 11.25h4M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z" />
                    </svg>

                    <span class="text-xs">Product</span>
                </div>
            </nuxt-link>

            <nuxt-link to="/scan">
                <div class="relative flex justify-center">
                    <button
                        class="bg-black rounded-full w-16 h-16 flex items-center justify-center text-white absolute -top-10 shadow-md border-4 border-white">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 128 128" id="QrCode">
                            <path stroke="#ffffff" stroke-linecap="round" stroke-linejoin="round" stroke-width="5"
                                d="M102 83L102 97C102 99.7614 99.7614 102 97 102L83 102M102 45L102 31C102 28.2386 99.7614 26 97 26L83 26M45 102L31 102C28.2386 102 26 99.7614 26 97L26 83M26 45L26 31C26 28.2386 28.2386 26 31 26L45 26"
                                class="colorStroke000000 svgStroke"></path>
                            <path stroke="#ffffff" stroke-linecap="round" stroke-width="5" d="M21 64H107"
                                class="colorStroke000000 svgStroke"></path>
                        </svg>
                    </button>
                </div>
            </nuxt-link>

            <nuxt-link v-if="userRole === 'admin'" to="/admin/employee">
                <div class="flex flex-col items-center text-gray-500">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" />
                    </svg>

                    <span class="text-xs">Employee</span>
                </div>
            </nuxt-link>



            <nuxt-link to="/picking">
                <div class="flex flex-col items-center text-gray-500">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M2.25 3h1.386c.51 0 .955.343 1.087.835l.383 1.437M7.5 14.25a3 3 0 0 0-3 3h15.75m-12.75-3h11.218c1.121-2.3 2.1-4.684 2.924-7.138a60.114 60.114 0 0 0-16.536-1.84M7.5 14.25 5.106 5.272M6 20.25a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0Zm12.75 0a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0Z" />
                    </svg>


                    <span class="text-xs">Picking</span>
                </div>

            </nuxt-link>

            <div v-if="userRole === 'employee'" @click="openModal" class="flex flex-col items-center text-gray-500">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" transform="rotate(180)"
                    stroke-width="1.5" stroke="currentColor" class="size-6">
                    <path stroke-linecap="round" stroke-linejoin="round"
                        d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15m-3 0-3-3m0 0 3-3m-3 3H15" />
                </svg>

                <span class="text-xs">Logout</span>
            </div>

        </footer>
    </div>
    <div @click="openModal" v-if="showModal"
        class="fixed inset-0 bg-gray-500 bg-opacity-75 flex items-center justify-center z-40">
        <div class="bg-white p-8 rounded-lg shadow-lg w-80">
            <div class="flex justify-center">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                    stroke="currentColor" class="size-20">
                    <path stroke-linecap="round" stroke-linejoin="round"
                        d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9 5.25h.008v.008H12v-.008Z" />
                </svg>

            </div>
            <h3 class="text-l text-center font-semibold mb-4">ต้องการออกจากระบบใช่หรือไม่ ?</h3>
            <div class="flex justify-between">
                <button @click="cancelLogout" class="px-4 py-2 bg-gray-400 text-white rounded">Cancel</button>
                <button @click="confirmLogout" class="px-4 py-2 bg-red-600 text-white rounded">Logout</button>
            </div>
        </div>
    </div>
</template>

<scrip></scrip>

<script>
import Cookies from 'js-cookie';

export default {

    data() {
        return {
            isDropdownOpen: false,
            isSidebarOpen: false,
            userRole: null,
            showModal: false,
        };
    },
    mounted() {
        this.userRole = Cookies.get('userRole');
        console.log('userRole:', this.userRole);
    },
    computed: {
        isScanPage() {
            return this.$route.path === "/scan"
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
        openModal() {
            this.showModal = true;
        },
        cancelLogout() {
            this.showModal = false;
        },
        confirmLogout() {
            Cookies.remove("token");
            Cookies.remove("userRole");
            this.$router.push("/login");
        },
    },
};

</script>

<style></style>