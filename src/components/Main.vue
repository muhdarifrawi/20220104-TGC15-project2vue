<template>
  <div class="container-fluid my-3">
    <div class="row">
      <div class="col-xl-2 col-xs">
        <div class="card mb-3" style="width: 14rem">
          <div class="card-body">
            <div class="form-floating mb-3">
            <input
              type="text"
              class="form-control"
              id="search-bar"
              placeholder="bicycle"
              v-model="searchInput"
              @input="searchEntry()"
            />
            <label for="floatingInput">Search word</label>
          </div>

          <div class="form-floating mb-2">
            <select
              class="form-select"
              id="search-by"
              aria-label="search-by label"
              v-model="searchMode"
            >
              <option value="title" selected>Item Name</option>
              <option value="category">Category</option>
              <option value="user">User</option>
              <option value="month">Month</option>
              <option value="year">Year</option>
            </select>
            <label for="floatingSelect">Search by</label>
          </div>

          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              id="cat-body"
              v-model="catBody"
            />
            <label class="form-check-label" for="cat-body"> Body </label>
          </div>

          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              id="cat-brakes"
              v-model="catBrakes"
            />
            <label class="form-check-label" for="cat-brakes"> Brakes </label>
          </div>

          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              id="cat-handles"
              v-model="catHandles"
            />
            <label class="form-check-label" for="cat-handles"> Handles </label>
          </div>

          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              id="cat-pedals"
              v-model="catPedals"
            />
            <label class="form-check-label" for="cat-pedals"> Pedals </label>
          </div>

          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              id="cat-seats"
              v-model="catSeats"
            />
            <label class="form-check-label" for="cat-seats"> Seats </label>
          </div>

          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              id="cat-tires"
              v-model="catTires"
            />
            <label class="form-check-label" for="cat-tires"> Tires </label>
          </div>
          </div>
          
        </div>
      </div>
      <div class="col-xl-10 col-xs">
        <div class="row row-cols-lg-4 row-cols-sm-1">
          <div class="col mx-3" v-for="items in database" :key="items.itemName">
            <div class="card mb-3" style="width: 18rem">
              <div class="card-body">
                <h5 class="card-title">{{ items.itemName }}</h5>
                <h6 class="grayed-text mt-2 mb-0 text-muted">
                  Submitted on {{ formatDate(items.date) }}
                </h6>
                <h6 class="grayed-text mt-0 text-muted">
                  under
                  <span class="text-capitalize">{{ items.category }}</span> by
                  {{ items.user }}
                </h6>
                <p class="card-text">
                  {{ items.itemDescription }}
                </p>

                <button
                  class="btn btn-outline-dark"
                  type="button"
                  data-bs-toggle="collapse"
                  :data-bs-target="`#options-${items._id}`"
                  aria-expanded="false"
                  :aria-controls="`options-${items._id}`"
                >
                  Options
                </button>
                <div class="collapse" :id="`options-${items._id}`">
                  <div
                    class="
                      card card-body
                      border-light
                      my-2
                      d-grid
                      gap-2
                      d-md-block
                    "
                  >
                    <a
                      href="#"
                      class="btn btn-danger mx-1"
                      v-on:click="deletingEntry(items._id)"
                      >Delete</a
                    >
                    <a
                      href="#"
                      class="btn btn-warning mx-1"
                      v-on:click="editingEntry(items._id)"
                      >Edit</a
                    >
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

// need to enable CORS on the express side
// npm install CORS? https://expressjs.com/en/resources/middleware/cors.html
console.log(process.env.VUE_APP_DEV_MONGO_URL);
export default {
  name: "Main",
  created: async function () {
    let response = await axios.get(process.env.VUE_APP_DEV_MONGO_URL);
    console.log(response.data);
    this.databaseActual = response.data;
    this.database = this.databaseActual;
  },
  data: function () {
    return {
      databaseActual: "",
      database: [],
      searchMode: "title",
      catBody:false,
      catBrakes:false,
      catHandles:false,
      catPedals:false,
      catSeats:false,
      catTires:false
    };
  },
  computed: {
    formatDate() {
      //moment(String(${dataDate})).format('YYYYMMDD')
      return (dataDate) => {
        let formattedDate = moment(String(dataDate)).format("Do MMMM YYYY");
        return formattedDate;
      };
    },
  },
  methods: {
    deletingEntry: function (_id) {
      this.$emit("delete-entry", _id);
    },
    editingEntry: function (_id) {
      this.$emit("edit-entry", _id);
    },
    searchEntry: function () {
      // console.log(this.database[0]["itemName"]);
      this.database = [];
      let searchType = "";
      // this.database.push(this.databaseActual[0])
      if (this.searchMode == "title") {
        searchType = "itemName";
      } else if (this.searchMode == "category") {
        searchType = "category";
      } else if (this.searchMode == "user") {
        searchType = "user";
      }

      this.database = this.databaseActual.filter((data) =>
        data[searchType].toLowerCase().includes(this.searchInput.toLowerCase())
      );
    },
  },
};
</script>

<style>
.grayed-text {
  font-size: 10pt;
}
</style>