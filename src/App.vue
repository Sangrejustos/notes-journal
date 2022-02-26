<template>
  <div class="app">
    <h1>Страница с заметками</h1>

    <div class = "app__btns">
      <my-button class="separateBtn" @click="showDialog">
        Создать заметку
      </my-button>
      <my-select
       v-model="selectedSort"
       :options="sortOptions "
       class = "b"
      />

    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>

    <post-list
        :posts="posts"
        @remove="removePost"
        v-if="!isLoading"
    />
  </div>
</template>

<script>
import PostList from "@/components/PostList";
import PostForm from "@/components/PostForm";
import axios from 'axios'
export default {
  components: {
    PostList, PostForm,
  },

  data() {
    return {
      posts: [],
      dialogVisible: false,
      isLoading: false,
      selectedSort: '',
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По содержимому'}
      ]
    }
  },

  methods: {
    createPost(Post) {
      this.posts.push(Post);
      this.dialogVisible = false;
    },
    removePost(Post) {
      this.posts = this.posts.filter(p => p.id !== Post.id)
    },
    showDialog() {
      this.dialogVisible = 'true'
    },
    async fetchNotes() {
      try {
        this.isLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=1')
        this.posts = response.data;
      } catch (e) {
        alert('Ошибка')
      } finally {
        this.isLoading = false
      }
    }
  },
  mounted() {
    this.fetchNotes();
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans+Condensed:wght@300&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Open Sans Condensed', sans-serif;
  color: teal;
  font-size: 26px;
}

.app {
  padding: 20px;
}

.separateBtn {

}

.app__btns {
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
}



</style>