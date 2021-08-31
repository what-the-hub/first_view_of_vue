<template>
  <form @submit.prevent>
    <h4>Creating post</h4>
    <input v-model="post.title" class="input_style" type="text" placeholder="name of post" />
    <input v-model="post.body" class="input_style" type="text" placeholder="description" />
    <button @click="createPost">Create</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: '',
      },
    };
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post);
      this.post = {
        title: '',
        body: '',
      };
    },
  },
};
</script>

<style scoped>
.input_style {
  width: 100%;
  margin: 20px;
}
</style>
