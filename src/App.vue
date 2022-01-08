<template>

  <Main />
  <AddListing />
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
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
