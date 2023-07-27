<template>
  <form @submit.prevent class="form">
    <h4>Создание поста</h4>
    <input
        v-model="post.title"
        type="text"
        placeholder="Название поста"
    >
    <input
        v-model="post.body"
        type="text"
        placeholder="Описание поста">
    <my-button
        @click="createPost"
        style="align-self: self-end; margin-top: 10px;">
      Создать
    </my-button>
  </form>
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
      this.post.id = Date.now();
      this.$emit('create',this.post);
      this.post = {
        title: '',
        body: '',
      }
    },
  }
}
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
}

</style>