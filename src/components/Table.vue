<template>
  <div id="table_test">
    <h1>{{ filterData.length }} items</h1>

    <loading v-if="reveal"></loading>

    <!-- search input  -->
    <input
      class="input is-normal my-5"
      type="text"
      placeholder="search a word"
      v-model="search"
    />

    <nav>
      <button type="button" v-if="page != 1" @click="page--">Previous</button>
      <span
        v-for="pageNumber in pages.slice(page - 1, page + 20)"
        @click="page = pageNumber"
        :key="pageNumber + 'pageNumber'"
      >
        <!-- display pagination button numbers -->
        <buttonPage :pageNumber="pageNumber" :page="page"></buttonPage>
      </span>
      <button type="button" @click="page++" v-if="page < pages.length">
        Next
      </button>
    </nav>

    <table class="myTable">
      <!-- Header of the table, clickable to sort -->
      <thead>
        <tr class="table-headers">
          <td
            v-for="(key, index) in header"
            :key="index + 'table'"
            @click="sort(key)"
          >
            <span>{{ key }}</span>
          </td>
        </tr>
      </thead>

      <!-- body of the table -->
      <tbody>
        <tr
          v-for="(row, indexTab) in displayedPosts"
          :key="indexTab + row.first_name + 'rowTr'"
        >
          <!-- modal you can click on any cell to display more detail -->
          <modal
            :reveal="reveal"
            :toggleModal="toggleModal"
            :object="row"
          ></modal>

          <td v-for="(value, index) in row" :key="index + 'rowValue'">
            <span v-if="index === 'id'" @click="toggleModal(row)"> ℹ️ </span>
            <!-- display cell with specific design -->
            {{ cleanValue(value, index) }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import MOCK_DATA from "../assets/MOCK_DATA.json";
import Modal from "./Modal";
import Loading from "./Loading";
import ButtonPage from "./ButtonPage";
// import Flag from './utils';
export default {
  name: "Table",
  components: {
    modal: Modal,
    loading: Loading,
    buttonPage: ButtonPage,
  },
  props: {
    msg: String,
  },
  mounted() {
    // restructuring json data to flat
    this.json = MOCK_DATA.map((i) => {
      let temp = i.customer;
      // removing custumer object because we jst flat it
      delete i.customer;
      return { ...i, ...temp };
    });
    this.header = Object.keys(this.json[0]);
  },
  data() {
    return {
      json: [],
      finalData: [],
      currentSort: "name",
      currentSortDir: "asc",
      search: "",
      header: [],
      reveal: false,
      obj: {},
      loading: false,
      options: {
        year: "numeric",
        month: "numeric",
        day: "numeric",
        hour: "numeric",
        minute: "numeric",
      },
      posts: [],
      page: 1,
      perPage: 50,
      pages: [],
    };
  },
  created: function () {},
  methods: {
    /**
     * Pagination according to the search term
     * set the number of page
     */
    setPages() {
      this.pages = Array.from(
        Array(Math.ceil(this.filterData.length / this.perPage)).keys()
      );
    },
    paginate(sortedData) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return sortedData.slice(from, to);
    },
    /**
     * function toogleModale(obj)
     * @param obj values of the row for display the modal
     */
    toggleModal: function (obj) {
      this.reveal = !this.reveal;
      // this.obj = obj;
      console.log(`obj`, obj);
    },
    isLoading() {
      console.log("Loading", this.loading);
      this.loading = !this.loading;
    },
    /**
     * function sort(s) : click on header to order the column
     * store the curent column sorted
     * @param s: string key of the header
     */
    sort: function (s) {
      console.log(`sort`, s);
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = s;
    },
    /**
     * Function Flag(isoCode) : Add a flag at the end of the Country code
     * ISO 3166-1 alpha-2
     * ⚠️ No support for IE 11
     * @param isoCode : String example: FR
     * @return 🇫🇷
     */
    Flag(isoCode) {
      return typeof String.fromCodePoint !== "undefined"
        ? isoCode
            .toUpperCase()
            .replace(/./g, (char) =>
              String.fromCodePoint(char.charCodeAt(0) + 127397)
            )
        : isoCode;
    },
    /**
     * Function cleanValue(value, i) : put some style to cell contain according to the header
     * @param value : String the value of the cell
     * @param i : String  the header
     * @return a string depending of the header
     */
    cleanValue(value, i) {
      if (
        i === "birthday" ||
        i === "due_date" ||
        i === "interaction_creation_date"
      )
        return new Date(value).toLocaleDateString("en-US", this.options);
      else if (i === "country_code") return value + this.Flag(value);
      else if (i === "last_comment") return value.slice(0, 10) + "...";
      else if (i === "status") {
        switch (value) {
          case "inProgress":
            return "⚙️" + value;
          // return <span class="badge badge-primary">"⚙️" + value</span>
          // return "⚙️" + value;
          case "waiting":
            return "🕑" + value;
          case "toTreat":
            return "❗️" + value;
          case "finished":
            return "✅" + value;
          case "reserved":
            return "🔓" + value;
          default:
            return value;
        }
      }
      return value;
    },
  },
  computed: {
    /**
     * function filterData()
     * loop into the json object,
     * loop into every row of the object
     * try to find match in every cell, if it find a match, the it return the row
     * not case sensitive
     */
    filterData: function () {
      // this.page = 1;
      return this.search
        ? // if you write something in the search bar
          this.json.filter((obj) => {
            let flag; // true if find a match
            // loop into the row object
            Object.values(obj).forEach((val) => {
              // loop into every cell of the row
              //try to find if the search is contained in the cell
              if (String(val).indexOf(this.search.toLowerCase()) > -1) {
                flag = true;
                return;
              }
            });
            // return the row if find a match
            if (flag) return obj;
          })
        : this.json;
    },
    /**
     * function sortedData
     * return the data order by the header selected
     */
    sortedData() {
      return this.filterData.slice(0).sort((a, b) => {
        let modifier = 1;
        if (this.currentSortDir === "desc") modifier = -1;
        if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      });
    },
    displayedPosts() {
      return this.paginate(this.sortedData);
    },
  },
  watch: {
    /**
     * recalculate the number of page according to the search result
     */
    filterData() {
      this.setPages();
    },
    /**
     * reset page number on search modification
     */
    search() {
      this.page = 1;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped src='./Table.css'>
</style>
