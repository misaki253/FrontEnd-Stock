<template>
  <div class="container max-w-full w-full p-0 lg:p-5">
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
    <div class="bg-gray-200 px-5 rounded-xl">
      <div class="mt-8 flow-root p-5">
        <table class="min-w-full divide-y divide-gray-300">
          <thead>
            <tr>
              <th
                scope="col"
                class="ml-5 pr-3 text-sm font-semibold text-gray-900 sm:pl-0"></th>
              <th
                scope="col"
                class="text-left ml-5 pr-3 text-sm font-semibold text-gray-900 sm:pl-0">
                Name
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 lg:table-cell">
                ID
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 lg:table-cell">
                Username
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 lg:table-cell">
                Password
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 lg:table-cell">
                Role
              </th>
              <th
                scope="col"
                class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 lg:table-cell">
                Role
              </th>
              <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-0">
                <span class="sr-only">Edit</span>
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200 bg-white">
            <tr v-for="person in paginatedPeople" :key="person.email">
              <td class="  pl-5">
                <div class="w-10">
                  <img class="rounded-full" :src="person.image" alt="" />
                </div>
              </td>
              <td
                class="whitespace-nowrap py-5 pr-10 text-sm sm:pl-0 sm:w-3">
                <div class="flex items-center ">
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
              <td
                class="hidden whitespace-nowrap px-3 py-5 text-sm text-gray-500 lg:table-cell">
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
                class="relative whitespace-nowrap py-5 pl-3 pr-4 text-sm font-medium sm:pr-0 w-5">
                <a href="#" class="text-indigo-600 hover:text-indigo-900"
                  >Edit</a
                >
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="flex justify-between items-center mt-4 bg-gray-200 p-3">
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
      perPage: 7,
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
