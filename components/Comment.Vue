<template>
    <div class="pb-1">
        <div class="comment-box">                  
            <PostUserInformation :userId="comment.user.id">
                <template v-slot:comment>
                    <div class="pt-2 comment-text">{{ comment.body }}</div>
                </template>
            </PostUserInformation>
        </div>
    </div>
</template>
<script>
import PostUserInformation from '@/components/PostUserInformation';
export default {
    name: "Comment",
    props: ['comment'],
    components: { PostUserInformation },
    mounted() {   
    },
}
</script>
<style scoped>
.comment-box{
    margin: 10px;
    border: 1px solid #90a4ae;
    padding: 9px;
    border-radius: 4px;
    margin-bottom:10px;
}
.comment-text{
    color:#000;
    font-weight: 500;
   
}
</style>