<template>
    <div class="mutual">
    <v-card v-for="mutual in mutuals" :key="mutual.userId" elevation="100" justify="center" outlined max-width="1200" height="160" style="width:400px;margin-top:200px;float:left;margin-left:20px;">
        <h4 style="text-align:center">People you may know</h4>
      <v-row justify="center">
      <v-avatar @click="toPost(post.postDTO.postId)" size= 80 style="height:70px;margin-top:20px; min-width: 80px; width: 80px;">
        <img :src="mutual.profileImage">
      </v-avatar>
    </v-row>
      <v-card-title @click="toProfile(mutual.userId)" class="text" style="font-size:15px;text-align:center;margin-left:150px">
        {{mutual.userName}}
        </v-card-title>
    </v-card>
</div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import jwtdecode from 'jwt-decode'
Vue.use(VueAxios, axios);
export default {
    name: "Mutuals",
    data(){
        return{
            userId: jwtdecode(localStorage.getItem("token")).userId,
            mutuals: undefined,
        }
    },
    methods: {
        toProfile(id){
            console.log(id)
            this.$store.state.friendId = id
            console.log(id)
            this.$router.push(this.$router.push({path: `/np/`}))
        },
    },
    mounted() {
        const uid = this.userId
        Vue.axios.get(`http://10.177.1.179:7081/pb/user/youMayKnow/${uid}`)
            .then((resp) => {
                this.mutuals = resp.data
                console.log("LOLLLLL");
                console.log(this.userId)
            })
        }
    }
</script>
<style>
.v-card__title {
    align-items: center;
    display: flex;
    flex-wrap: wrap;
    font-size: 1.25rem;
    font-weight: 500;
    letter-spacing: 0.0125em;
    line-height: 2rem;
    word-break: break-all;
    margin-left: 100px;
}

</style>