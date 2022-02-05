<template>
  <div class="card m-3" style="width: 18rem" v-for="items in database" :key="items.itemName">
    <div class="card-body">
      <h5 class="card-title">{{items.itemName}}</h5>
      <h6 class="card-subtitle mb-2 text-muted"> Submitted on  {{formatDate(items.date)}}</h6>
      <h6 class="card-subtitle mb-2 text-muted"> Under {{items.category}}</h6>
      <h6 class="card-subtitle mb-2 text-muted"> Submitted by {{items.user}}</h6>
      <p class="card-text">
          {{items.itemDescription}}
      </p>
      <a href="#" class="btn btn-danger mx-1" v-on:click="deletingEntry(items._id)">Delete</a>
      <a href="#" class="btn btn-warning mx-1">Edit</a>
    </div>
  </div>
  <!-- <p>{{database}}</p> -->
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
      database: "No entry so far. Be the first to upload!"
    };
  },
  computed: {
      formatDate(){
        //moment(String(${dataDate})).format('YYYYMMDD')
            return dataDate =>{
              let formattedDate = moment(String(dataDate)).format('Do MMMM YYYY');
              return formattedDate
            }
      }
  },
  methods:{
    deletingEntry:function(_id){
      console.log("Deleting entry triggered", _id)
      this.$emit("delete-entry",_id);
      // this.$emit("page","delete-entry")
    }
  }
};
</script>

<style>
</style>