<template>
  <div class="app">
    <h1>страница с заметками</h1>
    <my-button class="separeteBtn" @click="showDialog">
      Создать заметку
    </my-button>

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
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.separeteBtn {
  margin: 10px 0;
}

</style>