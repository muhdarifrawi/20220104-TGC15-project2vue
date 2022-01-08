<template>
    <p>{{database}}</p>
</template>

<script>
import axios from "axios"

// need to enable CORS on the express side
// npm install CORS? https://expressjs.com/en/resources/middleware/cors.html
console.log(process.env.VUE_APP_DEV_MONGO_URL);
export default {
    name:"Main",
    created: async function(){
        let response = await axios.get(process.env.VUE_APP_DEV_MONGO_URL);
        this.database = response.data;
    },
    data:function() {
     return{
        database:"this is a test"
     }   
    }
}
</script>

<style>

</style>