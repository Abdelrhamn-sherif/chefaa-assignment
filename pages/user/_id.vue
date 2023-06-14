<template>
    <div>            
      <Posts :url="userPostsUrl" />
    </div>
  </template>
  
  <script>
  import Posts from '@/components/Posts.vue';
  export default {
    name: 'userPage',
    components: { Posts },
    data(){
        return{
            userPostsUrl: `https://dummyjson.com/posts/user/${this.$route.params.id}`
        }
    }
  }
  </script>
  