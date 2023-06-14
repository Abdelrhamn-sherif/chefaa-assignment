<template>
    <div>      
        <v-skeleton-loader v-if="isLoading" class="mx-auto border mt-5" max-width="100%" type="card-heading" />
          
        <v-row align="start" class="mx-0" v-else>
            <v-col cols="12" md="2">
                <v-img height="60" :src="user.image" />
            </v-col>
            <v-col class="pt-1" cols="12" md="10">
                <v-row class="col-12">
                    
                    
                    <div class="black--text body-1 pe-2 pt-1">{{ fullName }}</div>
                </v-row>
                <NuxtLink class="black--text subtitle-2 pe-2 pt-1" target="_blank" :to="{ path: `/user/${user.id}`}">
                    <div class="black--text subtitle-2 pe-2 pt-1">

                        @{{ user.username }}
                    </div>
                </NuxtLink>
                
                <slot name="comment"></slot>
                <slot name="userComment"></slot>
            </v-col>
        </v-row>
    </div>
</template>
<script>
export default {
    name: "PostUserInformation",
    props: ['userId'],
    computed: {
        fullName: function () {
            return this.user.firstName + " " + this.user.lastName
        }
    },
    data(){
        return{
            user: {},
            isLoading:true
        }
    },
    async mounted() {
        const user = await this.$axios.get(`https://dummyjson.com/users/${this.userId}`).then(response => (this.user = response.data));
        this.isLoading = false
    },
}
</script>
<style></style>