<template>
  <div>
    <h1>Страница с заметками</h1>
    <my-input
        v-model="searchQuery"
        placeholder = "Поиск постов"
    />
    <div class = "app__btns">
      <my-button class="separateBtn" @click="showDialog">
        Создать заметку
      </my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      />

    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>

    <post-list
        :posts="searchPost"
        @remove="removePost"
        v-if="!isLoading"
    />
    <div ref="observer" class="observer"></div>
  </div>
  <!--  <div class="page__wrapper">-->
  <!--        <div-->
  <!--            v-for="pageNumber in totalPages"-->
  <!--            key="pageNumber"-->
  <!--            class="page"-->
  <!--            :class="{-->
  <!--              'current-page': page === pageNumber-->
  <!--            }"-->
  <!--            @click="changePage(pageNumber)"-->
  <!--        > {{pageNumber}}-->
  <!--        </div>-->
  <!--  </div>-->

</template>

<script>
import PostList from "@/components/PostList";
import PostForm from "@/components/PostForm";
import MyButton from "@/components/UI/MyButton";
import axios from 'axios'
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";

export default {
  components: {
    PostList,
    PostForm,
    MyButton,
    MySelect,
    MyInput,
  },

  data() {
    return {
      posts: [],
      dialogVisible: false,
      isLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 1,
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
    /*changePage(pageNumber) {
      this.page = pageNumber
    },*/
    async fetchNotes() {
      try {
        this.isLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert('Серверные данные не загружены')
      } finally {
        this.isLoading = false
      }
    },

    async loadMoreNotes() {
      try {
        this.page += 1
        this.isLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('Серверные данные не загружены')
      } finally {
        this.isLoading = false
      }
    },
  },

  mounted() {
    this.fetchNotes();
    const options = {
      rootMargin: '0px',
      threshold: 1.0
    }
    const callback = (entries, observer) => {
      if(entries[0].isIntersecting && this.page <  this.totalPages) {
        this.loadMoreNotes()
      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer);
  },

  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) =>
          post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      )},

    searchPost() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },

  watch: {
    // page() {
    //   this.fetchNotes()
    // }
  },

}
</script>

<style>


.separateBtn {

}

.app__btns {
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid teal;
  border-radius: 7%;
  padding: 10px;
  cursor: pointer;
}

.current-page {
  border: 3px solid teal;
}

.observer {
  height: 30px;
  background: green;
}
</style>