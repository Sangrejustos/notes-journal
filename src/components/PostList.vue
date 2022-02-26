<template>
  <div v-if="posts.length > 0">
    <h3>Список пользователей</h3>
    <post-item
        v-for="post in posts"
        :post="post"
        :key="post.id"
        @remove="$emit('remove', post)"
    />
  </div>
  <h3 v-else class="alert">
    Список заметок пуст
  </h3>
</template>

<script>
import PostItem from "@/components/PostItem";
export default {
  components: {PostItem},
  props: {
    posts: {
      type: Array,
      required: true,
    }
  }

}
</script>

<style scoped>
.alert {
  color:teal;
  text-align: left;
  margin: 10px 0;
}
</style>