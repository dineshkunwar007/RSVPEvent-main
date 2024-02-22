<template>
  <q-page class="flex">
    <div class="q-pa-md row items-start q-gutter-md" v-if="activeScreen=='list'" v-for="i in 3" :key="i">
      <q-card class="my-card" @click="onEventClick(i)" >
        <img src="https://cdn.quasar.dev/img/mountains.jpg" />

        <q-card-section>
          <div class="text-h6">Event summary</div>
          <div class="text-subtitle2">by Dinesh (Mymanu)</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          {{ lorem }}
        </q-card-section>
      </q-card>
      <!-- EVENT Details start-->
     
     <!-- EVENT Details end-->
    </div>
    <div v-else>
     Event NO. {{ selectedEvent }}
      <RegisterUser :name="activeScreen"></RegisterUser>
      <q-card  class="my-card"  >
      <img src="https://cdn.quasar.dev/img/parallax2.jpg">
      
      <q-list style="align-items:baseline;">
        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="primary" name="date_range" />
          </q-item-section>

          <q-item-section>
            <q-item-label>01/04/2024</q-item-label>
            <q-item-label caption>10:00AM - 3:00PM</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="primary" name="location_on" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Blue Tower</q-item-label>
            <q-item-label caption>Host Social, Media city M54UF Manchester</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="primary" name="share" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Share</q-item-label>
           
          </q-item-section>
        </q-item>
        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="primary" name="book_online" />
          </q-item-section>

          <q-item-section >
            <q-item-label>Book your ticket</q-item-label>
          
          </q-item-section>
        </q-item>
      </q-list>
    </q-card>
    
    </div>

  <!-- login/register strt-->
  <!-- <div class="column q-pa-lg">
      <div class="row">
        <q-card square class="shadow-24" style="width:300px;height:485px;">
          <q-card-section class="bg-deep-purple-7">
            <h4 class="text-h5 text-white q-my-md">Event Login</h4>
            <div class="absolute-bottom-right q-pr-md" style="transform: translateY(50%);">
              <q-btn fab icon="add" color="purple-4" />
            </div>
          </q-card-section>
          <q-card-section>
            <q-form class="q-px-sm q-pt-xl">
              <q-input square clearable v-model="email" type="email" label="Email">
                <template v-slot:prepend>
                  <q-icon name="email" />
                </template>
              </q-input>
              <q-input square clearable v-model="password" type="password" label="Password">
                <template v-slot:prepend>
                  <q-icon name="lock" />
                </template>
              </q-input>
            </q-form>
          </q-card-section>
          <q-card-section>
            <div class="text-center q-pa-md q-gutter-md">
              <q-btn round color="indigo-7">
                <q-icon name="fab fa-facebook-f" size="1.2rem" />
              </q-btn>
              <q-btn round color="red-8">
                <q-icon name="fab fa-google-plus-g" size="1.2rem" />
              </q-btn>
              <q-btn round color="light-blue-5">
                <q-icon name="fab fa-twitter" size="1.2rem" />
              </q-btn>
            </div>
          </q-card-section>
          <q-card-actions class="q-px-lg">
            <q-btn unelevated size="lg" color="purple-4" class="full-width text-white" label="Sign In" />
          </q-card-actions>
          <q-card-section class="text-center q-pa-sm">
            <p class="text-grey-6">Forgot your password?</p>
          </q-card-section>
        </q-card>
      </div>
    </div>
    <div class="column q-pa-lg">
      <div class="row">
        <q-card square class="shadow-24" style="width:300px;height:485px;">
          <q-card-section class="bg-deep-purple-7">
            <h4 class="text-h5 text-white q-my-md">Event Registration</h4>
            <div class="absolute-bottom-right q-pr-md" style="transform: translateY(50%);">
              <q-btn fab icon="close" color="purple-4" />
            </div>
          </q-card-section>
          <q-card-section>
            <q-form class="q-px-sm q-pt-xl q-pb-lg">
              <q-input square clearable v-model="email" type="email" label="Email">
                <template v-slot:prepend>
                  <q-icon name="email" />
                </template>
              </q-input>
              <q-input square clearable v-model="username" type="username" label="Username">
                <template v-slot:prepend>
                  <q-icon name="person" />
                </template>
              </q-input>
              <q-input square clearable v-model="password" type="password" label="Password">
                <template v-slot:prepend>
                  <q-icon name="lock" />
                </template>
              </q-input>
            </q-form>
          </q-card-section>
          <q-card-actions class="q-px-lg">
            <q-btn unelevated size="lg" color="purple-4" class="full-width text-white" label="Get Started" />
          </q-card-actions>
          <q-card-section class="text-center q-pa-sm">
            <p class="text-grey-6">Return to login</p>
          </q-card-section>
        </q-card>
      </div>
    </div> -->
    <!-- login/register end-->
<div class="q-pa-md row items-start q-gutter-md" >

  <div v-if="user" >{{ user.email}}<q-btn @click="signout">Sign Out</q-btn></div>
  <q-form @submit.prevent="submit">
  <div>
  <q-input v-model="email" type="email" placeholder="Enter Email">
  <template v-slot:prepend> <q-icon name="email" />  </template></q-input>
   </div>
  <div>  <q-input  v-model="password" type="password" rea placeholder="Enter password">
    <template v-slot:prepend> <q-icon name="password" />  </template></q-input>
 </div>
  <q-btn type="submit">{{ mode==='login' ? 'Login' : 'Register'}}</q-btn>
   <div @click="toggleMode(mode==='login' ? 'register' : 'login')">{{ mode==='login' ? 'Not a user? Register' : 'Already a user? Login'}}</div>
  </q-form>
</div>
    <q-footer rounded>
      <q-tabs
        no-caps
        active-color="primary"
        indicator-color="transparent"
        class="text-grey-8"
        v-model="tab"
      >
        <q-tab name="images"><img src="icons/50.png"></q-tab>
        <q-tab name="videos"> <img src="icons/panel.png"></q-tab>
        <q-tab name="articles"><img src="icons/profile.png"></q-tab>
      </q-tabs>
    </q-footer>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { event, useQuasar } from 'quasar'
import RegisterUser from 'src/components/RegisterUser.vue'
import {auth} from 'src/firebase.js'
import {createUserWithEmailAndPassword,signInWithEmailAndPassword,signOut, onAuthStateChanged} from 'firebase/auth'
import {ref} from 'vue'
export default ({
  name: "IndexPage",
  components:{
    RegisterUser,
  },
  data(){
    const $q = useQuasar()
    const mode= ref('Register')
    const user= ref(null)
     
  
   
    $q.dark.set(true)
    return {
       mode:ref('login'),
      email: '',
      username: '',
      password: '',
      activeScreen:'list',
      events:[],
      selectedEvent:null,
      lorem:
         "This is demo event for testing purposes only. These events will be driven from backend services dynamically.This is demo event fir testing urposes only. These events will be driven from backend services dynamically.",
     };
    }
    ,
    methods:{
    onEventClick(event){

      debugger;
      this.selectedEvent=event;
      this.activeScreen='details:'

    },
     submit(event)
    { // alert(JSON.stringify(event))
      //debugger;
    let email= this.email;
    let password= this.password;
    if(this.mode==='login')
    { signInWithEmailAndPassword(auth,email,password).then((res) => {
      if(res !=null && res!=undefined && res.user.uid !=null && res.user.uid!=undefined)
      alert("Login successful")
      }).catch((err)=>
      {alert(err.message)})
      onAuthStateChanged(auth, currentUser =>{
      this.user=currentUser
    })
    }
    else{ createUserWithEmailAndPassword(auth,email,password).then((res) => {
      if(res !=null && res!=undefined && res.user.uid !=null && res.user.uid!=undefined)
      alert("User registered successful")
      }).catch((err)=>
      {
     alert( err.message)})}
    
  },
   toggleMode(val){
      this.mode=val
           
    }   
   
  
  }
  /* ,
  mounted() {
    this.events= response;
  },
  beforeCreate(){

  } */
  });
 
  

</script>