<template>
<div>
      <h4 class="text-center mt-20">
       <small>
         <button class="btn btn-success" v-on:click="navigate()"> Посмотреть все посты </button>
       </small>
    </h4>
        <div class="col-md-12 form-wrapper">
          <h2> Редактирование поста</h2>
          <form id="edit-post-form" @submit.prevent="editPost">
            <div class="form-group col-md-12">
                <label for="title"> Заголовок </label>
                <input type="text" id="title" v-model="post.title" name="title" class="form-control" placeholder="Введите заголовок">
            </div>
            <div class="form-group col-md-12">
                <label for="description"> Описание </label>
                <input type="text" id="description" v-model="post.description" name="description" class="form-control" placeholder="Введите описание">
            </div>
            <div class="form-group col-md-12">
                <label for="body"> Контент </label>
                <textarea id="body" cols="30" rows="5" v-model="post.body" class="form-control"></textarea>
            </div>
            <div class="form-group col-md-12">
                <label for="author"> Автор </label>
                <input type="text" id="author" v-model="post.author" name="author" class="form-control">
            </div>

            <div class="form-group col-md-4 pull-right">
                <button class="btn btn-success" type="submit"> Изменить пост </button>
            </div>
          </form>
        </div>
    </div>
</template>

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
    editPost() {
      let postData = {
        title: this.post.title,
        description: this.post.description,
        body: this.post.body,
        author: this.post.author,
        date_posted: this.post.date_posted
      };

      axios
        .put(`${server.baseURL}/blog/edit?postID=${this.id}`, postData)
        .then(data => {
          router.push({ name: "home" });
        });
    },
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

