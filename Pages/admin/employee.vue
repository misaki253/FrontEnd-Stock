<template>
  <div class="container max-w-full w-full p-5">
    <div class="sm:flex sm:items-center sm:justify-between">
      <div class="sm:flex-auto">
        <h1 class="text-lg font-semibold text-gray-900">Users</h1>
        <p class="mt-2 text-sm text-gray-700">
          A list of all the users in your account including their name, title,
          email, and role.
        </p>
      </div>
      <div class="mt-4 sm:ml-16 sm:mt-0 sm:flex-none">
        <button
          type="button"
          class="block rounded-md bg-indigo-600 px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
          Add user
        </button>
      </div>
    </div>
    <div class="bg-gray-200 px-5 rounded-xl py-5">

      <div class="bg-white rounded p-2">
        <div class="w-80 ">
        <form class="">
          <div class="relative">
            <div
              class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
              <svg
                class="w-4 h-4 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 20">
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
              </svg>
            </div>
            <input
              type="search"
              id="default-search"
              class="block w-full p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              placeholder="Search Mockups, Logos..."
              required />
            <button
              type="submit"
              class="text-white absolute end-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
              Search
            </button>
          </div>
        </form>
      </div>
      </div>

      <div class="flow-root mt-5">
        <table class="min-w-full divide-y divide-gray-300">
          <thead class="bg-black text-white">
            <tr>
              <th
                scope="col"
                class="ml-5 pr-3 text-sm font-semibold sm:pl-0"></th>
              <th
                scope="col"
                class="text-left ml-5 pr-3 text-sm font-semibold sm:pl-0">
                Name
              </th>
              <th
                scope="col"
                class="px-3 py-3.5 text-left text-sm font-semibold">
                ID
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold lg:table-cell">
                Username
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold lg:table-cell">
                Password
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold lg:table-cell">
                Role
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold lg:table-cell">
                Status
              </th>
              <th
                scope="col"
                class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:pl-0 hidden lg:table-cell"></th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200 bg-white">
            <tr
              v-for="person in paginatedPeople"
              :key="person.email"
              class="cursor-pointer hover:bg-gray-100">
              <td class="pl-5">
                <div class="w-10">
                  <img class="rounded-full" :src="person.image" alt="" />
                </div>
              </td>
              <td class="whitespace-nowrap py-5 pr-10 text-sm sm:pl-0 sm:w-3">
                <div class="flex items-center">
                  <div class="">
                    <div class="font-medium text-gray-900">
                      {{ person.name }}
                    </div>
                    <div class="mt-1 text-gray-500 sm:hidden">
                      {{ person.role }}
                    </div>
                    <div class="text-gray-500 truncate sm:hidden">
                      {{ person.email }}
                    </div>
                  </div>
                </div>
              </td>
              <td class="whitespace-nowrap px-3 py-5 text-sm text-gray-500">
                {{ person.uid }}
              </td>
              <td
                class="hidden whitespace-nowrap px-3 py-5 text-sm text-gray-500 lg:table-cell">
                {{ person.username }}
              </td>
              <td
                class="hidden whitespace-nowrap px-3 py-5 text-sm text-gray-500 lg:table-cell">
                {{ person.password }}
              </td>
              <td
                class="hidden whitespace-nowrap px-3 py-5 text-sm text-gray-500 lg:table-cell">
                {{ person.role }}
              </td>
              <td
                class="whitespace-nowrap px-3 py-5 text-sm text-gray-500 hidden lg:table-cell">
                <span
                  :class="[
                    'inline-flex items-center rounded-md px-2 py-1 text-xs font-medium ring-1 ring-inset ',
                    person.status === 'Active'
                      ? 'bg-green-50 text-green-700 ring-green-600/20 '
                      : 'bg-red-50 text-red-700 ring-red-600/20 ',
                  ]"
                  >{{ person.status }}</span
                >
              </td>
              <td
                class="flex relative whitespace-nowrap py-5 text-sm font-medium hidden sm:pr-0 lg:flex">
                <!--edit-->
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-6 text-black">
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                </svg>

                <!--delete-->
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-6 ml-5 text-red-500">
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                </svg>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="flex justify-between items-center mt-4 bg-white p-3">
        <div>
          รายการที่ {{ startIndex + 1 }} ถึง {{ endIndex }} จากทั้งหมด
          {{ people.length }} รายการ
          <select v-model="perPage" @change="currentPage = 1">
            <option v-for="size in [5, 10, 15, 20]" :key="size" :value="size">
              {{ size }} รายการต่อหน้า
            </option>
          </select>
        </div>

        <div class="flex items-center">
          <button
            @click="goToPage(currentPage - 1)"
            :disabled="currentPage === 1"
            class="btn mx-1">
            &lt;
          </button>
          <span>Page {{ currentPage }} of {{ totalPages }}</span>
          <button
            @click="goToPage(currentPage + 1)"
            :disabled="currentPage === totalPages"
            class="btn mx-1">
            &gt;
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage: 1,
      perPage: 5,
      people: [
        {
          name: "Lind1say Walton",
          uid: "U00001",
          username: "admin",
          password: "a@1234",
          status: "Active",
          role: "Member",
          image:
            "https://images.unsplash.com/photo-1517841905240-472988babdf9?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
        },
        {
          name: "Lind1say Walton",
          uid: "U00001",
          username: "admin",
          password: "a@1234",
          status: "Active",
          role: "Member",
          image:
            "https://images.unsplash.com/photo-1517841905240-472988babdf9?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
        },
      ],
    };
  },
  computed: {
    paginatedPeople() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = this.currentPage * this.perPage;
      console.log(`Start: ${start}, End: ${end}, Total: ${this.people.length}`);
      return this.people.slice(start, end);
    },

    totalPages() {
      return Math.ceil(this.people.length / this.perPage);
    },
    startIndex() {
      return (this.currentPage - 1) * this.perPage;
    },
    endIndex() {
      return Math.min(this.startIndex + this.perPage, this.people.length);
    },
    pages() {
      return Array.from({ length: this.totalPages }, (_, i) => i + 1);
    },
  },
  methods: {
    goToPage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page;
      }
    },
  },
};
</script>
