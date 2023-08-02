<template>
  <form class="form" @submit.prevent>
    <h4>Создание поста</h4>
    <my-input
        v-focus
        v-model="post.title"
        type="text"
        placeholder="Название поста"
    />
    <my-input
        v-model="post.body"
        type="text"
        placeholder="Описание поста"
    />
    <my-button
        @click="createPost"
        style="align-self: self-end; margin-top: 10px;"
    >
      Создать
    </my-button>
  </form>
</template>

<script>
import MyButton from "@/components/UI/MyButton.vue";
import MyInput from "@/components/UI/MyInput.vue";

export default {
  components: {MyInput, MyButton},
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