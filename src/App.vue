<template>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a
            v-on:click="goProductReviews"
            class="nav-link active"
            aria-current="page"
          >
            Product Reviews
          </a>
        </li>
        <li class="nav-item">
          <a v-on:click="goProductEntry" class="nav-link">
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
  <Main v-if="page==='product-review'"/>
  <AddListing v-if="page==='product-entry'" />
</div>
  
</template>

<script>
import Main from "@/components/Main.vue";
import AddListing from "@/components/AddListing.vue";

export default {
  name: "App",
  components: {
    Main,
    AddListing,
  },
  data:function(){
    return{
      "page":"product-review"
    }
  },
  methods: {
    goProductReviews: function () {
      this.page="product-review"
    },
    goProductEntry: function () {
      this.page="product-entry"
    },
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
a:hover{
  cursor: pointer;
}
</style>
