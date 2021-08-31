<template>
  <div class="post">
      <div>
          <strong>Name:</strong> {{post.title}}
      </div>
      <div>
          <strong>Description: </strong> {{post.body}}
      </div>
      <button @click="$emit('remove', post)">Delete</button>
    <!-- <h3>List of Users</h3>
    <post-item v-for="post in posts" :post="post">1</post-item> -->
  </div>
</template>
<script>
// import PostItem from '@/components/PostItem.vue';

export default {
//   components: { PostItem },
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>
<style scoped>
.post {
  margin: 20px;
  border: 1px solid teal;
}
</style>
