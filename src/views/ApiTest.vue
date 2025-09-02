<template>
  <div>{{ msg }}</div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      msg: "API Test",
    };
  },
  mounted() {
    axios //
      .post({ url: "api/productList", data: {} })
      .then((result) => console.log(result));
  },
};
</script>

<style>
/*  */
</style>
