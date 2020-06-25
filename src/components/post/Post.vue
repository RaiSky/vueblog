<template>
    <div class="text-center">
        <div class="col-sm-12">
      <h4 style="margin-top: 30px;"><small><button class="btn btn-success" v-on:click="navigate()"> View All Posts </button></small></h4>
      <hr>
      <h2 class="v-title">{{ post.title }}</h2>
      <h5><span class="glyphicon glyphicon-time"></span> Создатель поста {{post.author}}, {{post.date_posted}}.</h5>
      <p class="post-body"> {{ post.body }} </p>
  
    </div>
    </div>
</template>

<style>
  .post-body {
    margin: auto;
    width: 960px;
    text-align: justify;
  }
</style>
<script>
import { server } from "../../utils/helper";
import axios from "axios";
import router from "../../router";
export default {
  data() {
    return {
      id: 0,
      post: {}
    };
  },
  created() {
    this.id = this.$route.params.id;
    this.getPost();
  },
  methods: {
    getPost() {
      axios
        .get(`${server.baseURL}/blog/post/${this.id}`)
        .then(data => (this.post = data.data));
    },
    navigate() {
      router.go(-1);
    }
  }
};
</script>



