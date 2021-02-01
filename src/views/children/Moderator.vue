<template>
<div>
  <h2 class="bus" >MODERATOR PROFILES</h2>
  
    <v-text-field class="blue lighten-4--text" dark hide-details single-line v-model="val" style="float:left;width:500px;margin:10px 10px 10px 350px;border:1px solid #006499;"></v-text-field>
      <v-btn icon @click="submit">
        <v-icon style="color:black;height:200px">mdi-magnify</v-icon>
      </v-btn>
  
    <v-card  class="frnd" elevation="100" justify="center" outlined max-width="7500" v-for="res in results" :key="res.userId">
      <v-row justify="center">
      <v-avatar size= 100>
        <img :src="res.profileImage" alt="John" class="img">
      </v-avatar>
    </v-row>
      <v-card-title class="text" style="">
        {{res.userName}}
        </v-card-title>
    <v-btn class="follow" rounded color="#006499" @click="remove(res.userId)">
        REMOVE
      </v-btn>
    </v-card>
</div>
</template>
<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios);
import jwtdecode from 'jwt-decode'
export default {
  name: 'Request',
  data() {
    return {
      val: "",  
      userId: jwtdecode(localStorage.getItem("token")).userId,
      results: undefined,
      removeModerator:{
        businessId: "",
        moderatorId: ""
        
      }
    }
  },
  mounted(){
    const uid = jwtdecode(localStorage.getItem("token")).userId
    Vue.axios.get(`http://10.177.1.179:7081/pb/user/allModerators/${uid}`)
      .then((resp) => {
        this.results = resp.data
        console.log(resp.data)
      })
  },
  methods: {
      submit(){
        console.log(this.val)
        this.$store.state.moderatorId = this.val
        this.$router.push({path: `/searchmod/`})
      },
      remove(id){
          this.removeModerator.moderatorId = id
          this.removeModerator.businessId = this.userId
          console.log(this.removeModerator)
          Vue.axios.post('http://10.177.1.179:7081/pb/user/removeModerator1', {
            "moderatorId":id,
            "businessId":this.userId
          })
        .then((resp) => {
          console.log(resp.data)
        }).catch((resp)=>{
          console.log(resp);
        }
        )
      }
  }
}
</script>

<style>
.img{
  border-radius: 20px;
  margin-top: 30%;
}
.frnd{
  margin-top: 50px;
  width: 500px;
  margin-left: 40%;
}
.bus{
  text-align:center;
  margin-left: 300px;
  margin-top: 20px;

}
.text{
  text-align: center;
  margin-left: 190px;
}
</style>