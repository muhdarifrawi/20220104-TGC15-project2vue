<template>
  <div class="container">
    <h1>Product Entry</h1>
    <form class="form-floating needs-validation" novalidate>
    <div class="form-floating mb-2">
      <input
        type="date"
        name="entry-date"
        id="entry-date"
        class="form-control"
        :class="{'is-invalid':dateEmpty}"
        placeholder="John Doe"
        v-model="date"
        required
      />
      <label for="entry-date">Entry Date</label>
    </div>

    <div class="form-floating mb-2">
      <input
        type="text"
        name="user"
        id="user"
        class="form-control"
        :class="{'is-invalid':userEmpty}"
        placeholder="John Doe"
        v-model="user"
        required
      />
      <label for="user">User</label>
    </div>

    <div class="form-floating mb-2">
      <input
        type="text"
        name="item-name"
        id="item-name"
        class="form-control"
        :class="{'is-invalid':itemNameEmpty}"
        placeholder="John Doe"
        v-model="itemName"
        required
      />
      <label for="entry-date">Item Name</label>
    </div>

    <div class="form-floating mb-2">
      <select
        class="form-select"
        :class="{'is-invalid':categoryEmpty}"
        id="category"
        aria-label="category label"
        v-model="category"
        required
      >
        <option value="">Choose a category</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select>
      <label for="floatingSelect">Category</label>
    </div>

    <div class="form-floating mb-2">
      <textarea
        class="form-control"
        :class="{'is-invalid':itemDescriptionEmpty}"
        placeholder="Fill in description"
        id="item-description"
        v-model="itemDescription"
        required
      ></textarea>
      <label for="item-description">Item Description</label>
    </div>

    <button type="button" class="btn btn-primary" v-on:click="checkForm">
      Submit
    </button>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "AddListing",
  emits:["errorStatus"],
  data: function () {
    return {
      date: null,
      user: null,
      itemName: null,
      category: "",
      itemDescription: null,
      dateEmpty: false,
      userEmpty: false,
      itemNameEmpty: false,
      categoryEmpty: false,
      itemDescriptionEmpty: false,
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
    },
    newProduct: function () {
      let date = this.date;
      let user = this.user;
      let itemName = this.itemName;
      let category = this.category;
      let itemDescription = this.itemDescription;

      console.log(date, user, itemName, category, itemDescription);
    },
    processAdd: async function () {
      // console.log("mongo url: " + process.env.VUE_APP_DEV_MONGO_URL);
      try {
        let response = await axios.post(process.env.VUE_APP_DEV_MONGO_URL + "product/", {
          date:this.date,
          user:this.user,
          itemName:this.itemName,
          category:this.category,
          itemDescription:this.itemDescription
        });
        console.log(response.data);
      }
      catch(e){
        console.log(e.request.status);
        let errorStatus = e.request.status;
        if(errorStatus == 404){
          this.$emit("errorStatus","Network error (404)")
        }
      }
      
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