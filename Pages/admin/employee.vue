<script>
import axios from 'axios';
import Cookies from "js-cookie";

export default {
  data() {
    return {
      users: [],
      currentPage: 1,
      perPage: 5,
      searchQuery: "",
      loading: false,
      token: Cookies.get("token"),
    };
  },
  computed: {
    filteredUsers() {
      return this.users.filter(
        (user) =>
          user.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          user.uid.toString().includes(this.searchQuery)
      );
    },
    paginatedUsers() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = this.currentPage * this.perPage;
      return this.filteredUsers.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.filteredUsers.length / this.perPage);
    },
    pages() {
      return Array.from({ length: this.totalPages }, (_, i) => i + 1);
    },
    startIndex() {
      return (this.currentPage - 1) * this.perPage;
    },
    endIndex() {
      return Math.min(this.startIndex + this.perPage, this.filteredUsers.length);
    },
  },
  methods: {
    getdata() {
      this.loading = true;
      axios
        .get("https://project-stock.onrender.com/api/users", {
          headers: {
            Authorization: `Bearer ${this.token}`,
          },
        })
        .then((response) => {
          this.users = response.data.data;  // Ensure 'data' is correct based on API response structure
          this.loading = false;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
          this.loading = false;
        });
    },
    goToPage(page) {
      if (page < 1 || page > this.totalPages) return;
      this.currentPage = page;
    },
  },
  mounted() {
    this.getdata();  // Call the getdata method when the component is mounted
  },
  watch: {
    searchQuery() {
      this.currentPage = 1; // Reset to the first page when the search query changes
    },
  },
};
</script>
