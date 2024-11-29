<template>
  <div class="nav">
    <div class="nav-in">
      <div class="logo">Логотип</div>
      <div class="right-group">
        <div class="name">Имя, фамилия</div>
        <div class="menu-dropdown">MENU</div>
      </div>
    </div>
  </div>
  <div class="app">
    <post-list :posts="posts" @remove="removePost" v-if="!isPostsLoading" />

    <div v-else>Идет загрузка...</div>
    <div class="app__btns">
      <my-button @click="showDialog"> Новый осмотр </my-button>
      <my-select />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>
  </div>
</template>

<script>
import PostForm from "@/Components/PostForm";
import PostList from "@/Components/PostList";
import axios from "axios";
import MyButton from "@/Components/UI/MyButton.vue";

export default {
  components: {
    PostForm,
    PostList,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get(
          "http://26.181.201.144:8000/patient/get_one/5"
        );
        this.posts = response.data;
      } catch (e) {
        alert("Ошибка");
      } finally {
        this.isPostsLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app__btns {
  display: flex;
  justify-content: space-between;
}

.nav {
  width: 100%;
  height: 50px;
  background-color: teal;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
}

.nav-in {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  width: 100%;
}

.right-group {
  display: flex;
  align-items: center;
  gap: 15px;
}
</style>
