<template>
    <div class="ad" style="float: right" >
        <div v-for="items in ads" :key="items.id">
            <v-img class="adimg" max-height="300" max-width="300"
                :src="items.imageUrl" style="margin:35px;margin-left:600px"></v-img>
                <!-- <br> -->
                <!-- <b><h5>{{items.title}}</h5></b>
                <h6>{{items.description}}</h6> -->
        </div>
    
    </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import jwtdecode from 'jwt-decode'
Vue.use(VueAxios, axios);
export default {
    name: "Ads",
    data(){
        return{
            userId: jwtdecode(localStorage.getItem("token")).userId,
            ads: [],
            user: {}
            
            // stories: undefined,
        }
    },
    // mounted() {
    //     const uid = this.userId
    //     Vue.axios.get(`http://10.177.1.69:8081/pb/story/getFriendsStories/${uid}`)
    //         .then((resp) => {
    //         this.stories = resp.data
    //         console.log("LOLLLLL");
    //         console.log(resp.data)
    //         })
    // }
    mounted() {
        console.log("Hello world")
        this.user.username = jwtdecode(localStorage.getItem("token")).username;
        this.user.channelId = jwtdecode(localStorage.getItem("token")).channelId;
        axios
        .get(`http://10.177.1.104:8085/user/findByChannelIdAndUsername/${this.user.channelId}/${this.user.username}`)
        .then((resp) => resp.data)
        .then((data) => {
        console.log(data.type);
        this.user = data;
        if (data) {
        console.log(this.user.userId);
        axios
        .get("http://10.177.1.134:8000/ad/user/" + `${this.userId}`)
        // .get("http://10.177.1.134:8000/ad/sleep,reebok")
        .then((res) => {
            console.log("Aaaaaddddddsssssss")
            console.log(res.data)
            this.ads = res.data
            console.log(this.ads)
        })
        }
        });
 }
}
</script>