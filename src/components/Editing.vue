<template>
  <div class="container">
    <h1>Edit Product Info</h1>
    <div class="form-floating mb-2">
      <input
        type="date"
        name="entry-date"
        id="entry-date"
        class="form-control"
        placeholder="John Doe"
        v-model="date"
      />
      <label for="entry-date">Entry Date</label>
    </div>

    <div class="form-floating mb-2">
      <input
        type="text"
        name="user"
        id="user"
        class="form-control"
        placeholder="John Doe"
        v-model="user"
      />
      <label for="user">User</label>
    </div>

    <div class="form-floating mb-2">
      <input
        type="text"
        name="item-name"
        id="item-name"
        class="form-control"
        placeholder="John Doe"
        v-model="itemName"
      />
      <label for="entry-date">Item Name</label>
    </div>

    <div class="form-floating mb-2">
      <select
        class="form-select"
        id="category"
        aria-label="category label"
        v-model="category"
      >
        <option value="">Choose a category</option>
        <option value="brakes">Brakes</option>
        <option value="pedals">Pedals</option>
        <option value="body">Body</option>
        <option value="seats">Seats</option>
        <option value="handles">Handles</option>
        <option value="tires">Tires</option>
      </select>
      <label for="floatingSelect">Category</label>
    </div>

    <div class="form-floating mb-2">
      <textarea
        class="form-control"
        placeholder="Fill in description"
        id="item-description"
        v-model="itemDescription"
      ></textarea>
      <label for="item-description">Item Description</label>
    </div>

    <button type="button" class="btn btn-warning" v-on:click="checkForm">
      Edit
    </button>
    <a href="#" class="btn btn-outline-dark mx-1" v-on:click="cancel()">Cancel</a>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "EditListing",
  props: ["id"],
  created: async function () {
    console.log(this.id);
    let response = await axios.get(
      process.env.VUE_APP_DEV_MONGO_URL + "products/" + this.id
    );
    console.log(response.data);
    this.date = moment(String(response.data.result.date)).format("YYYY-MM-DD");
    this.user = response.data.result.user;
    this.itemName = response.data.result.itemName;
    this.category = response.data.result.category;
    this.itemDescription = response.data.result.itemDescription;
  },
  data: function () {
    return {
      date: "",
      user: "",
      itemName: "",
      category: "",
      itemDescription: "",
    };
  },
  methods: {
    checkForm:function(){
      this.$emit("clearError");
      this.dateEmpty = false;
      this.userEmpty = false;
      this.itemNameEmpty = false;
      this.categoryEmpty = false;
      this.itemDescriptionEmpty = false;

      if(!this.date){
        console.log("date e");
        this.dateEmpty = true;
        this.$emit("errorStatus","Empty date field")
      }
      if(!this.user){
        console.log("user e");
        this.userEmpty = true;
        this.$emit("errorStatus","Empty user field")
      }
      if(!this.itemName){
        console.log("itemName e");
        this.itemNameEmpty = true;
        this.$emit("errorStatus","Empty item name field")
      }
      if(!this.category){
        console.log("category e");
        this.categoryEmpty = true;
        this.$emit("errorStatus","Empty category field")
      }
      if(!this.itemDescription){
        console.log("itemDescription e");
        this.itemDescriptionEmpty = true;
        this.$emit("errorStatus","Empty item description field")
      }
      if(!this.itemDescriptionEmpty && !this.categoryEmpty && !this.itemNameEmpty && !this.userEmpty && !this.dateEmpty){
        console.log("All filled ");
        this.processEdit();
      }
    },
    processEdit: async function () {
      // console.log("mongo url: " + process.env.VUE_APP_DEV_MONGO_URL + "products/" + this.id);
      try {
        let response = await axios.post(
        process.env.VUE_APP_DEV_MONGO_URL + "products/" + this.id,
        {
          _id: this.id,
          date: this.date,
          user: this.user,
          itemName: this.itemName,
          category: this.category,
          itemDescription: this.itemDescription,
        }
      );
      console.log(response.data);
      }
      catch(e){
        console.log(e);
      }
      
    },
    cancel:function(){
      this.$emit("product-review");
    },
  },
  
};
</script>

<style>
#item-description {
  height: 150px;
  resize: none;
}
</style>