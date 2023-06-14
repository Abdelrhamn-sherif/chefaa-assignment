<template >
    <div>
        <div class="comments-section">
            <div class="mt-3 mx-3" v-if="comments.total > limit && comments.total != 1">
                <div class="blue--text text-decoration-underline pointer" @click="loadMoreCommentsClick()">View more
                    comments
                </div>
            </div>           
            <div class="my-3 mx-3" v-if="comments.total == 0 && comments.comments?.length == 0">
                There is no comments to show
            </div>
            <Comment v-for="comment in comments.comments" :comment="comment" :key="comment.id" />
        </div>
        <div class="pt-3">

            <v-fade-transition>
                <PostUserInformation :userId="1" v-if="toggleComment">
                    <template v-slot:userComment>
                        <v-form class="my-3 mx-3" ref="form" v-model="valid" lazy-validation @submit="commentSubmit">
                            <v-row align="center">
                                <v-textarea v-model="form.comment" :rules="form.commentRules"
                                    placeholder="Type your comment" no-resize rows="2" @keyup.enter="commentSubmit"
                                    required></v-textarea>
                                <v-btn type="submit" :disabled="!valid" color="primary" class="ms-4 mr-4" @click="validate">
                                    Comment
                                </v-btn>
                            </v-row>
                        </v-form>

                    </template>
                </PostUserInformation>
            </v-fade-transition>
        </div>
    </div>
</template>
<script>
import Comment from '@/components/comment';
export default {
    name: "Comments",
    props: ['commentsId', 'toggleComment'],
    components: { Comment },
    data() {
        return {
            comments: [],
            limit: 2,
            valid: false,
            form: {
                comment: "",
                commentRules: [
                    v => !!v || 'Comment is required',
                ],
            }
        }
    },
    async mounted() {
        await this.loadMoreComments();
    },
    methods: {
        async loadMoreComments() {
            const comments = await this.$axios.get(`https://dummyjson.com/comments/post/${this.commentsId}?skip=0&limit=${this.limit}`).then(response => (this.comments = response.data));
        },
        async loadMoreCommentsClick() {
            this.limit += 3
            await this.loadMoreComments()
        },
        commentSubmit(e) {
            e.preventDefault();
            this.comments.comments.unshift(
                {
                    body: this.form.comment,
                    user: {
                        id: 1,

                    }
                }
            )
            this.form.comment = ""
        },
        validate() {
            this.$refs.form.validate()
        },
    },
}
</script>
<style>
.comments-section {
    max-height: 490px;
    overflow-y: auto;
}
</style>