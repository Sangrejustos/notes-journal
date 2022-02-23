<template>
  <div>
    <form @submit.prevent>
      <h4>Создать пост</h4>
      <my-input
          v-model="post.title"
          @input="post.title = $event.target.value"
          class="input"
          type="text"
          placeholder="Название"
      ></my-input>
      <my-input
          v-model="post.body"
          @input="post.body = $event.target.value"
          class="input"
          type="text"
          placeholder="Описание"
      ></my-input>
      <my-button
          class="btn makePost"
          @click="createPost"
      >
        Выложить
      </my-button>
    </form>
  </div>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
export default {
  components: {MyInput},
  data() {
    return {
      post: {
        title: '',
        body: '',
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post)
      this.post = {
        body: '',
        title: '',
      }
    }
  }
}
</script>

<style scoped>

form {
  display: flex;
  flex-direction: column;
}

.makePost {
  align-self: flex-end;
  margin-top: 15px;
}
</style>