<template>
    <div>
      <div class="text-center">
        <h1>My Blog</h1>
       <p> Блог был создан с помощью  Vue.js, Nest.js и MongoDB</p>

       <div v-if="posts.length === 0">
            <h2> На данный момент посты отстутствуют </h2>
        </div>
      </div>
       <div class="container">
         <div class="row">
           <div class="col-md-4" v-for="post in posts" :key="post._id">
             <div class="card mb-4 shadow-sm">
               <div class="card-body">
                 <h2 class="card-img-top">{{ post.title }}</h2>
                 <p class="card-text">{{ post.body }}</p>
                 <div class="d-flex justify-content-between align-items-center">
                   <div class="btn-group" style="margin-bottom: 20px;">
                     <router-link :to="{name: 'Post', params: {id: post._id}}" class="btn btn-sm btn-outline-secondary btn-show">Просмотреть </router-link>
                     <router-link :to="{name: 'Edit', params: {id: post._id}}" class="btn btn-sm btn-outline-secondary btn-edit">Изменить</router-link>
                     <button class="btn btn-sm btn-outline-secondary btn-del" v-on:click="deletePost(post._id)">Удалить</button>
                   </div>
                 </div>

                 <div class="card-footer">
                   <small class="text-muted text-muted-style">Дата создания: {{ post.date_posted}}</small><br/>
                   <small class="text-muted text-muted-style">Автор: {{ post.author}}</small>
                 </div>

               </div>
             </div>
           </div>
         </div>
       </div>
    </div>
</template>

<style>
  .row {
    display: flex;
    flex-wrap: wrap;
  }
  .card {
    border-radius: 20px;
    width: 300px;
    height: auto;
    margin-left: 10px;
    border: 3px solid darkcyan;
  }
  .card-body {
    border-radius: 20px;
  }
  .btn {
    color: black;
  }
  .btn-show:hover {
      background-color: #42b983;
  }
  .btn-edit:hover {
    background-color: darkgoldenrod;
  }
  .btn-del:hover {
    background-color: darkred;
  }
  .card-img-top {
    height: auto;
    overflow: hidden;
    text-align: justify;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .card-text {
    height: auto;
    overflow: hidden;
    text-align: justify;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .card-footer {
    border-top: 5px solid darkcyan;
    background-color: #2c3e50;

  }
  .card-footer:last-child {
    border-bottom-right-radius: 20px;
    border-bottom-left-radius: 20px;
  }
  .card-footer >.text-muted-style{
    color: #fff !important;
  }
</style>

<script>
// @ is an alias to /src
import { server } from "@/utils/helper";
import axios from "axios";

export default {
  data() {
    return {
      posts: []
    };
  },
  created() {
    this.fetchPosts();
  },
  methods: {
    fetchPosts() {
      axios
        .get(`${server.baseURL}/blog/posts`)
        .then(data => (this.posts = data.data));
    },
    deletePost(id) {
      axios.delete(`${server.baseURL}/blog/delete?postID=${id}`).then(data => {
        console.log(data);
        window.location.reload();
      });
    }
  }
};
</script>

