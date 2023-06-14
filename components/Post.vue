<template >
    <div class="pt-5">

        <v-skeleton-loader v-if="isLoading" class="mx-auto border mt-5" max-width="100%" type="card" />
        <v-card class="mx-auto mt-5" v-else>
            <PostUserInformation :userId="post.userId" />
            <v-card-title class="pb-2">{{ post.title }}</v-card-title>
            <v-card-text class="subtitle-1 black--text" @click="toggleTextLength = !toggleTextLength">
                {{ postContent }}
            </v-card-text>
            <div class="subtitle-2 ms-1">
                <v-icon class="ms-3 pe-1 subtitle-1  blue--text text--darken-2">mdi-thumb-up</v-icon> {{
                    post.reactions }}
            </div>
            <v-divider class=" mx-3 mt-3"></v-divider>
            <v-row>
                <v-col cols="12">
                    <v-card-text>
                        <div class="mx-3">
                            <v-row justify="space-between">
                                <div class="py-2 pointer" @click="toggleLike">
                                    <v-icon :color="isLiked ? 'primary' : ''">mdi-thumb-up</v-icon>
                                    <span class="ms-1">Like</span>
                                </div>
                                <div class="py-2 pointer" @click="toggleComment = !toggleComment">
                                    <v-icon>mdi-comment</v-icon>
                                    <span class="ms-1">Comment</span>
                                </div>
                                <div class="py-2 pointer" @click="linkDialog = true">
                                    <v-icon>mdi-share</v-icon>
                                    <span class="ms-1">Share</span>
                                </div>
                            </v-row>
                        </div>
                    </v-card-text>
                </v-col>
            </v-row>
            <v-divider class=" mx-3"></v-divider>

            <Comments :commentsId="post.id" :toggleComment="toggleComment" />
        </v-card>
        <v-dialog v-model="linkDialog" width="500" >
            <v-card>
                <v-card-text class="subtitle-2 pt-4">
                    {{ postUrl }}
                    <div class="pt-3">
                        <v-btn color="primary" @click=" copyModalLink()">copy url</v-btn>
                    </div>
                </v-card-text>
            </v-card>
        </v-dialog>
    </div>
</template>
<script>
import PostUserInformation from "@/components/PostUserInformation"
import Comments from "@/components/Comments"
export default {
    name: "Post",
    components: { PostUserInformation, Comments },
    props: ["post", "currentUser"],
    data() {
        return {
            isLiked: false,
            isLoading: true,
            valid: true,
            linkDialog: false,
            toggleComment: false,
            postUrl:`http://custom-url/${this.post.id}`,
            toggleTextLength: true
        }
    },
    async mounted() {
        this.isLoading = false;
    },
    methods: {
        toggleLike() {
            this.isLiked ? this.post.reactions -= 1 : this.post.reactions += 1;
            this.isLiked = !this.isLiked;
        },
        async copyModalLink() {
            try {
                await navigator.clipboard.writeText(this.postUrl);                
            } catch ($e) {
                alert('Cannot copy');
            }
        }

    },
    computed: {
        postContent: function () {
            return this.post.body.length > 50 && this.toggleTextLength ? this.post.body.substring(0, 50) + '...' : this.post.body
        }
    }
}
</script>
