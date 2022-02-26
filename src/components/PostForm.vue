<template>
  <div>
    <form @submit.prevent>
      <h4>Создать заметку</h4>
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
export default {
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
      this.post = {
        id: Date.now(),
        body: this.post.body,
        title: this.post.title,
      }
      this.$emit('create', this.post);
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