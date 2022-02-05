<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Navbar</a>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <a
              v-on:click="goProductReviews"
              class="nav-link"
              v-bind:class="{ active: page === 'product-review' }"
              aria-current="page"
            >
              Product Reviews
            </a>
          </li>
          <li class="nav-item">
            <a
              v-on:click="goProductEntry"
              class="nav-link"
              v-bind:class="{ active: page === 'product-entry' }"
            >
              Product Entry
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Pricing</a>
          </li>
          <li class="nav-item">
            <a class="nav-link">About Us</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <div>
    <Main v-if="page === 'product-review'" v-on:delete-entry="onDeleteEntry"  v-on:edit-entry="onEditEntry"/>
    <AddListing v-if="page === 'product-entry'" />
    <Deleting v-if="page === 'delete-entry'" v-bind:id="currentEntry" v-on:product-review="goProductReviews"/>
    <Editing v-if="page === 'edit-entry'" v-bind:id="currentEntry"/>
  </div>
</template>

<script>
import Main from "@/components/Main.vue";
import AddListing from "@/components/AddListing.vue";
import Deleting from "@/components/Deleting.vue";
import Editing from "@/components/Editing.vue";

export default {
  name: "App",
  components: {
    Main,
    AddListing,
    Deleting,
    Editing
  },
  data: function () {
    return {
      page: "product-entry",
      currentEntry: "0"
    };
  },
  methods: {
    goProductReviews: function () {
      this.page = "product-review";
    },
    goProductEntry: function () {
      this.page = "product-entry";
    },
    onDeleteEntry: function(_id){
      console.log("onDeleteEntry triggered");
      this.page = "delete-entry";
      this.currentEntry = _id;
    },
    onEditEntry: function(_id){
      console.log("onEditEntry triggered");
      this.page = "edit-entry";
      this.currentEntry = _id;
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
a:hover {
  cursor: pointer;
}
</style>
