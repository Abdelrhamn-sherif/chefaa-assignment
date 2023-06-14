<template>
    <div>
        <v-row justify="center" align="center">
            <v-col cols="12" sm="8" md="6">
                <Post v-for="post in posts" :key="post.id" :post="post" />
            </v-col>
        </v-row>
        <div class="text-center mt-3 black--text h6 font-weight-bold" v-if="maxPosts == posts.length">No New Posts</div>
    </div>
</template>
<script>
import Post from '@/components/Post.vue'
export default {
    name: "Posts",
    components: { Post },
    props: {
        url: {
            type: String,
            default: "https://dummyjson.com/posts"
        }
    },
    data() {
        return {
           
            posts: [],
            currentPostsLength: 0,
            skip: 0,
            limit: 3,
            maxPosts: null,
            isLoading: false
        }
    },
    methods: {
        scroll() {
            window.onscroll = () => {
                let bottomOfWindow = Math.max(window.pageYOffset, document.documentElement.scrollTop, document.body.scrollTop) + window.innerHeight >= (document.documentElement.offsetHeight - 1);                
                if (bottomOfWindow && !this.isLoading && this.posts.length != this.maxPosts) {
                    this.limit += 1;
                    this.loadPosts();
                }
            }
        },
        async loadPosts() {
            if (!this.isLoading) {
                this.isLoading = true
                let posts = await this.$axios.get(`${this.url}?skip=${this.skip}&limit=${this.limit}`);
                this.posts = posts.data.posts;
                this.maxPosts = posts.data.total;
                this.isLoading = false;
            }
        }
    },
    async mounted() {        
        await this.loadPosts();
      
        this.currentPostsLength = this.posts.length;
        this.scroll();
    }

}
</script>
