<template>
  <div>
    <div v-if="posts.length > 0">
      <h3>List of Users</h3>
      <transition-group name="post-list">
        <post-item
          v-bind:key="post.id"
          v-for="post in posts"
          :post="post"
          @remove="$emit('remove', post)"
        />
      </transition-group>
      <!-- <post-item v-bind:key="post" v-for="post in posts" :post="post"></post-item> -->
    </div>
    <h2 v-else style="color: red">Emply list</h2>
  </div>
</template>

<script>
// import PostItem from '@/components/PostItem.vue';
import PostItem from './PostItem.vue';
// import PostItem from './PostItem.vue';
export default {
  components: { PostItem },
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
.post-list-item {
  display: inline-block;
  margin-right: 10px;
}
.post-list-enter-active,
.post-list-leave-active {
  transition: all 1s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.post-list-move {
  transition: transform 0.4s ease;
}
</style>
