<template>
  <div class="container">
    <div class="row d-flex justify-content-around">
      <div
    class="card m-3"
    style="width: 18rem"
    v-for="items in database"
    :key="items.itemName"
  >
    <div class="card-body">
      <h5 class="card-title">{{ items.itemName }}</h5>
      <div class="accordion accordion-flush" :id="`accordionFlush-${items._id}-1`">
        <div class="accordion-item my-1">
        <h6 class="accordion-header" :id="`flush-heading-${items._id}-1`">
          <button
            class="accordion-button"
            type="button"
            data-bs-toggle="collapse"
            :data-bs-target="`#flush-collapse-${items._id}-1`"
            aria-expanded="false"
            :aria-controls="`flush-collapse-${items._id}-1`"
          >
            Description
          </button>
        </h6>
        <div
          :id="`flush-collapse-${items._id}-1`"
          class="accordion-collapse collapse show"
          :aria-labelledby="`flush-heading-${items._id}-1`"
          :data-bs-parent="`#accordionFlush-${items._id}-1`"
        >
          <div class="accordion-body">
            {{ items.itemDescription }}
          </div>
        </div>
      </div>
      </div>
      <div class="accordion accordion-flush" :id="`accordionFlush-${items._id}-2`">
        <div class="accordion-item my-1">
        <h6 class="accordion-header" :id="`flush-heading-${items._id}-2`">
          <button
            class="accordion-button collapsed"
            type="button"
            data-bs-toggle="collapse"
            :data-bs-target="`#flush-collapse-${items._id}-2`"
            aria-expanded="false"
            :aria-controls="`flush-collapse-${items._id}-2`"
          >
            More..
          </button>
        </h6>
        <div
          :id="`flush-collapse-${items._id}-2`"
          class="accordion-collapse collapse"
          :aria-labelledby="`flush-heading-${items._id}-2`"
          :data-bs-parent="`#accordionFlush-${items._id}-2`"
        >
          <div class="accordion-body">
            <h6 class="card-subtitle mb-2 text-muted">
              Submitted on {{ formatDate(items.date) }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              Categorised under {{ items.category }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              Submitted by {{ items.user }}
            </h6>
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
    this.database = response.data;
  },
  data: function () {
    return {
      database: "No entry so far. Be the first to upload!",
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
  },
};
</script>

<style>
</style>