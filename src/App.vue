<!-- eslint-disable -->
<template>
  <div class="app">
    <button @click="showDialog">create post</button>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"></post-form>
    </my-dialog>
    <my-select v-model="selectedSort" :options="sortOptions"></my-select>
    <input v-model="searchQuery" type="text" placeholder="Search by name" />
    <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="isPostLoading !== true">
    </post-list>
    <!-- <div v-else>Loading../</div> -->
    <div class="page_wrapper">
      <div
        class="page"
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        :class="{
          current_page: page === pageNumber,
        }"
      >
        {{ pageNumber }}
      </div>
    </div>
    <div ref="obserever" class="observer"></div>
  </div>
</template>
<script>
import axios from 'axios';
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import MyDialog from './components/MyDialog.vue';
import MySelect from './components/MySelect.vue';

export default {
  components: {
    PostForm,
    PostList,
    MyDialog,
    MySelect,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        { value: 'title', name: 'name title' },
        { value: 'body', name: 'name body' },
      ],
    };
  },
  methods: {
    createPost(post) {
      console.log(post);
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async loadMorePosts() {
      try {
        this.isPostLoading = true;

        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          },
        });
        this.totalPages = Math.ceil(response.headers['x-tota-count'] / this.limit);

        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('something wrong');
      } finally {
        this.isPostLoading = false;
      }
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true;

        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          },
        });
        this.totalPages = Math.ceil(response.headers['x-tota-count'] / this.limit);

        this.posts = response.data;
      } catch (e) {
        alert('something wrong');
      } finally {
        this.isPostLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
    console.log(this.$refs.obserever);
    const options = {
      rootMargin: '0px',
      threshold: 1.0,
    };
    const callback = (entries, obserever) => {
      if (entries[0].isIntersecting) {
        this.loadMorePosts();
      }
    };
    const obserever = new IntersectionObserver(callback, options);
    obserever.observe(this.$refs.obserever);
  },
  computed: {
    sortedPosts() {
      const a = this.selectedSort;
      return [...this.posts].sort((post1, post2) => post1[a]?.localeCompare(post2[a]));
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) => post.title.includes(this.searchQuery));
    },
  },
  // watch: {
  //   selectedSort(newValue) {
  //     this.posts.sort((post1, post2) => post1[newValue]?.localeCompare(post2[newValue]));
  //   },
  // },
};
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.page_wrapper {
  height: 20px;
  display: flex;
  margin-top: 15px;
}
.page {
  background-color: grey;
  border: 1px solid rgb(75, 64, 47);
  padding: 10px;
}
.current_page {
  border: 3px solid green;
}
.observer {
  height: 50px;
  background: rgb(5, 75, 5);
}
</style>
