<template>
  <div class="container">
    <h1>Confirm Deletion</h1>
    <p>Delete the following entry?</p>
    <div class="card m-3" style="width: 18rem">
    <div class="card-body">
      <h5 class="card-title">{{database.result.itemName}}</h5>
      <h6 class="card-subtitle mb-2 text-muted"> Submitted on  {{formatDate(database.result.date)}}</h6>
      <h6 class="card-subtitle mb-2 text-muted"> Under {{database.result.category}}</h6>
      <h6 class="card-subtitle mb-2 text-muted"> Submitted by {{database.result.user}}</h6>
      <p class="card-text">
          {{database.result.itemDescription}}
      </p>
      <a href="#" class="btn btn-danger mx-1" v-on:click="deleteEntry(database.result._id)">Confirm</a>
      <a href="#" class="btn btn-outline-dark mx-1" v-on:click="cancel()">Cancel</a>
    </div>
  </div>
    {{ database }}
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

// need to enable CORS on the express side
// npm install CORS? https://expressjs.com/en/resources/middleware/cors.html
console.log(process.env.VUE_APP_DEV_MONGO_URL);
export default {
  name: "Deleting",
  props: ["id"],
  created: async function () {
    console.log(this.id);
    let response = await axios.get(
      process.env.VUE_APP_DEV_MONGO_URL + "products/" + this.id
    );
    console.log(response.data);
    this.database = response.data;
  },
  data: function () {
    return {
      // IMPORTANT: database should be set to null so that Vue can render properly.
      // SOURCE: https://stackoverflow.com/questions/54974846/prop-passed-to-child-component-is-undefined-in-created-method
      database: null,
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
    deleteEntry: async function () {
      try {
        await axios.delete(
          process.env.VUE_APP_DEV_MONGO_URL + "products/" + this.id
        );
      }
      catch(e){
        let errorStatus = e.request.status;
        if(errorStatus == 404){
          this.$emit("errorStatus","Network error (404)")
        }
      } 
      finally {
        this.$emit("product-review");
      }
    },
    cancel:function(){
      this.$emit("product-review");
    }
  },
};
</script>

<style>
</style>