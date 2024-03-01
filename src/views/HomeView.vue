<template>
    <div class="hello">
        <br>
        <h1>Facebook Login</h1>
        <input placeholder="input app id" v-model="inputId"/>
    
        <facebookLogin class="button"
        v-if="inputId"
         :appId="inputId"
          @login="getUserData"
          @logout="onLogout"
          :loginOptions="{
            enable_profile_selector: true,
        }">
        </facebookLogin>
        <input v-on:change="handleValue" :value="this.userToken" />
        <button v-on:click="handleToken" >Token</button>
        <div v-if="this.userToken"><div>
            <button v-on:click="getUserPosts">Get User Posts</button>
        </div>
        <div>
            <button v-on:click="getUserGroups">Get Groups</button>
        </div>
    </div>
        
    </div>
    </template>
     
    <script>
    import facebookLogin from 'facebook-login-vuejs';
    import axios from 'axios'
     
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        components: {
            facebookLogin
        },
        data: function () {
            return {
                userToken:'',
                markers: {
                    lat: 28.4595,
                    lng: 77.0266
                },
                userDetails:{},
                inputId:null,
    
            }
        },
        methods:{
            getUserData(data){
                console.log('---->', data.response)
                if(data.response.authResponse){
                    this.userToken = data.response.authResponse.accessToken
                    this.userDetails = data.response.authResponse
                }
                console.log('userDetails: ', this.userDetails)
            },
            handleToken(){
            //  console.log('userToken: ', this.userToken)   
            },
            handleValue(e){
             this.userToken = e.target.value
            },
            getUserPosts(){
                axios
          .get(`https://graph.facebook.com/v18.0/${this.userDetails.userID}/accounts?access_token=${this.userToken}`)
          .then(resp => {
            console.log('response: ', resp)
          }).catch((err)=>{
            console.log('error!!!!!: ', err)
          })
            },
            onLogout(resp){
                console.log('onLogout: ', resp)
                if(!resp.authResponse){
                    this.userDetails={}
                    this.userToken=null
                }
            },
            getUserGroups(resp){
     
            axios
            .get(`https://graph.facebook.com/v18.0/${this.userDetails.userID}/accounts?access_token=${this.userToken}`)
            .then(resp => {
                console.log('response: ', resp)
            }).catch((err)=>{
                console.log('error!!!!!: ', err)
            })
     
            }
        }
        ,
        mounted() {
            // At this point, the child GmapMap has been mounted, but
            // its map has not been initialized.
            // Therefore we need to write mapRef.$mapPromise.then(() => ...)
     
            // this.$refs.mapRef.$mapPromise.then((map) => {
            //     map.panTo({
            //         lat: 1.38,
            //         lng: 103.80
            //     })
            // })
        }
     
    }
    </script>
     
     
    <style scoped>
    h3 {
        margin: 40px 0 0;
    }
     
    ul {
        list-style-type: none;
        padding: 0;
    }
     
    li {
        display: inline-block;
        margin: 0 10px;
    }
    </style>
     
    