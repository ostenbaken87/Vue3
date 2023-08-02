<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input
        v-model="searchQuery"
        placeholder="Search..."
        v-focus
    >
    </my-input>
    <div class="app__btns">
      <my-button
          @click="showDialog">
        Создать пост
      </my-button>

      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      >
      </my-select>
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>

    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"
    />
    <div v-else>Loading...</div>
    <div v-intersection="loadMorePosts" class="observer"></div>
    <!--    <div class="page__wrapper">-->
    <!--      <div-->
    <!--          v-for="pageNumber in totalPages"-->
    <!--          :key="pageNumber"-->
    <!--          class="page"-->
    <!--          :class="{'current-page': page === pageNumber}"-->
    <!--          @click="changePage(pageNumber)"-->
    <!--      >-->
    <!--        {{pageNumber}}-->
    <!--      </div>-->
    <!--    </div>-->
  </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
import {entries} from "core-js/internals/object-to-array";

export default {
  components: {
    MyInput,
    MySelect,
    MyButton,
    MyDialog,
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'for title'},
        {value: 'body', name: 'for body'},
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true;
    },
    // changePage(pageNumber) {
    //   this.page = pageNumber
    // },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
        this.isPostsLoading = false;
      } catch (e) {
        alert('Error')
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
        this.isPostsLoading = false;
      } catch (e) {
        alert('Error')
      }
    }
  },
  mounted() {
    this.fetchPosts();
    console.log(this.$refs.observer);
    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPages) {
    //     this.loadMorePosts()
    //   }
    // }
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer);
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    },
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // },
  },
}

</script>

<style>


.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid teal;
  padding: 10px;
  color: teal;
  cursor: pointer;
}

.current-page {
  border: 2px solid teal;
  font-weight: bold;
  color: black;
}

.observer {
  height: 30px;
  background: teal;
}
</style>
