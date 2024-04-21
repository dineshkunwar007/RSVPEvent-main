<template>

  <q-page class="flex">
    <div>
  <div v-if="user">Signed In User: {{ user }}</div>
  <div v-if="loginstatus=='true'">
        <q-btn @click="handleSignOut()" >Sign Out</q-btn>
      </div>
</div>
<!-- style="position: absolute;
right: 10px;
top: 5px;" 
        -->
    <div v-if="loginstatus=='false'"  style="position: fixed;
    left:50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width:max-content;
    height:max-content">
    
     
        <q-card class="my-card">
          <!-- <q-card-section class="bg-deep-purple-7">
            <h4 class="text-h5 text-white q-my-md"> Please login or register below </h4>
            
          </q-card-section> -->
          <q-card-section>
            <q-form  @submit.prevent="submit" class="q-px-sm q-pt-xl">
              <q-input square clearable v-model="email" type="email"  label="Email">
                <template v-slot:prepend>
                  <q-icon name="email" />
                </template>
              </q-input>
              <q-input square clearable v-model="password" type="password" label="Password">
                <template v-slot:prepend>
                  <q-icon name="lock" />
                </template>
              </q-input>
              <br><br>
              <q-card-actions class="q-px-lg">
            <q-btn type="submit" unelevated size="lg" color="purple-4" class="full-width text-white">{{
          mode === "login" ? "Login" : "Register"
        }}</q-btn>
           
          </q-card-actions>
            </q-form>
            
          </q-card-section>
          
          <q-card-section>
            <div class="text-center q-pa-md q-gutter-md">
             
              <q-btn class="transparent" @click="googleSignIn()" >
              
                <img src="icons/icons8-google-48.png"/>           </q-btn>
              <q-btn @click="twitterSignIn()" >
                <img src="icons/icons8-twitter-48.png"/>
              </q-btn>
              <q-btn @click="facebookSignIn()">
                <img src="icons/icons8-facebook-logo-48.png"/>
              </q-btn>
            </div>
          </q-card-section>
          
          <q-card-section class="text-center q-pa-sm">
            <!-- <p class="text-grey-6">Forgot your password?</p> -->
            <div class="text-grey-6" style="-webkit-text-fill-color:grey;text-decoration-color: chocolate;" @click="toggleMode(mode === 'login' ? 'register' : 'login')">
          {{
            mode === "login" ? "Don't have an account? Register here":"Already a user? Login"
          }}
        </div>
          </q-card-section>
        </q-card>
        
     
    </div> 
    <div v-else>
    <div
      class="q-pa-md row items-start q-gutter-md"
      v-if="activeScreen == 'list'"
      v-for="objEvent in computedEventList"
      :key="objEvent.id"
    >
      <q-card class="my-card" @click="onEventClick(objEvent)">
        <img :src="objEvent.eventImg" />

        <q-card-section>
          <div class="text-h6">{{ objEvent.eventTitle }}</div>
          <div class="text-subtitle2">{{ objEvent.createdby }}</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          {{ objEvent.eventSummary }}
        </q-card-section>
      </q-card>
      <!-- EVENT Details start-->

      <!-- EVENT Details end-->
    </div>
    <div id="dvdetail" v-else>
      <RegisterUser :name="activeScreen"></RegisterUser>
      <div class="">
        <q-card>
    <q-carousel
      swipeable
      animated
      v-model="slide"
      :autoplay="autoplay"
      ref="carousel"
      infinite
    >
      <q-carousel-slide :name="1" img-src="https://firebasestorage.googleapis.com/v0/b/event-app-5d176.appspot.com/o/img.jpg?alt=media&token=546b6d33-c513-444e-ad56-d892dee94028"/>
      <q-carousel-slide :name="2" img-src="https://firebasestorage.googleapis.com/v0/b/event-app-5d176.appspot.com/o/demo.jpg?alt=media&token=41ac12ce-8687-423c-a5d1-9ffc59ca626a" />
      <q-carousel-slide :name="3" img-src="https://firebasestorage.googleapis.com/v0/b/event-app-5d176.appspot.com/o/ces.jpg?alt=media&token=6e88e5a5-10b3-4cc3-b12d-80b3a6293f2a" />
      <q-carousel-slide :name="4" img-src="https://firebasestorage.googleapis.com/v0/b/event-app-5d176.appspot.com/o/HYD03944.jpg?alt=media&token=3e1a1faf-d7af-472a-af42-28329374bc00" />
      <q-carousel-slide :name="4" img-src="https://firebasestorage.googleapis.com/v0/b/event-app-5d176.appspot.com/o/HYD03946.jpg?alt=media&token=99b4f9dc-77c4-40a8-a56f-e204ae7a4c11" />

      <template v-slot:control>
        <q-carousel-control
          position="top-right"
          :offset="[18, 18]"
          class="text-white rounded-borders"
          style="background: rgba(0, 0, 0, .3); padding: 4px 8px;"
        >
          <q-toggle dense dark color="orange" v-model="autoplay" label="Auto Play" />
        </q-carousel-control>

        <q-carousel-control
          position="bottom-right"
          :offset="[18, 18]"
          class="q-gutter-xs"
        >
          <q-btn
            push round dense color="orange" text-color="black" icon="arrow_left"
            @click="$refs.carousel.previous()"
          />
          <q-btn
            push round dense color="orange" text-color="black" icon="arrow_right"
            @click="$refs.carousel.next()"
          />
        </q-carousel-control>
      </template>
    </q-carousel></q-card>
  </div>
      <q-card class="my-card">
        <!-- <img src="https://cdn.quasar.dev/img/parallax2.jpg" /> -->

        <q-list style="align-items: baseline">
        <q-item> <q-item-section>
              
              <q-item-label caption
                >{{selectedEvent.eventTitle}}</q-item-label
              >
            </q-item-section>
          </q-item>
          <q-item> <q-item-section>
              
              <q-item-label caption
                >{{selectedEvent.eventSummary}}</q-item-label
              >
            </q-item-section>
          </q-item>
          <q-item> <q-item-section>
            
              <q-item-label caption
                >{{"Contact person: "+selectedEvent.contactPerson}}</q-item-label
              >
            </q-item-section>
          </q-item>
          <q-item> <q-item-section>
             
              <q-item-label caption
                >{{"Contact number: "+selectedEvent.contactPhone}}</q-item-label
              >
            </q-item-section>
          </q-item>
          <q-item> <q-item-section>
             
              <q-item-label caption
                >{{"Tickets remaining: "+selectedEvent.totalseat}}</q-item-label
              >
            </q-item-section>
          </q-item>
          <q-item clickable>
            <q-item-section avatar>
              <q-icon color="primary" name="date_range" />
            </q-item-section>

            <q-item-section>
              <q-item-label>{{selectedEvent.eventDate}}</q-item-label>
              <q-item-label caption>{{selectedEvent.eventTime}}</q-item-label>
            </q-item-section>
          </q-item>

          <q-item clickable>
            <q-item-section avatar>
              <q-icon color="primary" name="location_on" />
            </q-item-section>

            <q-item-section>
              <q-item-label>Event venue</q-item-label>
              <q-item-label caption
                >{{selectedEvent.eventLocation}}</q-item-label
              >
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

            <q-item-section>
              <q-item-label @click="Bookticket('yes')">Book your ticket</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card>
    </div>
  </div>   
   <div v-if="isbooking=='true'" class="q-pa-md">
    <q-card class="my-card">
    <q-form
      @submit="onSubmit"
      @reset="onReset"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="BookEvent.AttendeeName"
        label="Your name *"
    
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type your full name']"
      />
       <q-select filled v-model="BookEvent.Gender" 
      :options="options" label="Gender"
    
       />
       <q-input type="email"
        filled
        v-model="BookEvent.Personemail"
        label="Your email address *"
     
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type your email adddress']"
      />
      <q-input
        filled
        type="number"
        v-model="BookEvent.PhoneNumber"
        label="Contact number *"
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Please type your contact number',
        
        ]"
      />
      <q-input
        filled
        v-model="BookEvent.Dietpreference"
        label="Do you have any diet preference ?"/>
        <q-input
        filled
        type="number"
        v-model="BookEvent.AccompaniedBy"
        label="How many people are you accompanying with ?"
        lazy-rules
        :rules="[
          
          val => val < 5 || 'Maximum of 4 people can be accompanied with.'
        
        ]"
      />

      <q-toggle v-model="accept" label="I agree and consent to receiving updates about the event." />

      <div style="float:right">
        <q-btn label="Book a ticket" type="submit" color="primary"/>
        <q-btn label="Reset the entries" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
    </q-form>
    </q-card>
  </div>  
  <div id="dvqr" style="margin:auto; width: 50%">
        <canvas id="qr-code"></canvas>
        <q-btn square  label="share" @click="shareCanvas()">
                <template v-slot:prepend>
                  <q-icon name="share" />
                </template>
              </q-btn>
        
  </div>
   <div> <q-footer rounded v-if="activeScreen == 'list'||activeScreen == 'details:' ">
      <q-tabs
        no-caps
        active-color="primary"
        indicator-color="transparent"
        class="text-grey-8"
        v-model="tab"
      >
        <q-tab @click="onHomeClick" name="images"><img src="icons/50.png" /></q-tab>
        <q-tab name="videos"> <img src="icons/panel.png" /></q-tab>
        <q-tab name="articles"><img src="icons/profile.png" /></q-tab>
      </q-tabs>
    </q-footer></div>
 
  </q-page>
  
</template>

<script>
import { defineComponent } from "vue";
import { event, useQuasar,Notify } from "quasar";
import RegisterUser from "src/components/RegisterUser.vue";
import { auth } from "src/firebase.js";
import {
  db,
  collection,
  storage,
  sRef,
  uploadBytesResumable,
  getDownloadURL,
} from "src/firebase.js";
import { addDoc, onSnapshot, doc, updateDoc } from "firebase/firestore";
import {
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
  signOut,getAuth,
  onAuthStateChanged,GoogleAuthProvider,signInWithPopup,FacebookAuthProvider,TwitterAuthProvider,signInWithRedirect
} from "firebase/auth";
import { ref } from "vue";
import { FirebaseError } from "firebase/app";
import QRious from "qrious";
export default {
  name: "IndexPage",
  components: {
    RegisterUser,
  },
  data() {
    const $q = useQuasar();
    const mode = ref("Register");
    const user = ref(null);
    const eventList = ref([]);
    const activeScreen = ref("list");
    const loginstatus= ref("false");
    const accept = ref(false)
    const BookEvent={
      /* AttendeeName:"Deepak",
      PhoneNumber:"0123131263",
      Personemail:"danny@mymanu.co.uk",
      Gender:"Male",
      Dietpreference:"NA",
      AccompaniedBy:"4" */
    };
    const slide= ref(1);
    const  autoplay= ref(true);
    const isbooking=ref("false");
    
    $q.dark.set(true);
    return {
      mode: ref("login"),
      BookEvent:{
      AttendeeName:"",
      PhoneNumber:"",
      Personemail:"",
      Gender:"",
      Dietpreference:"",
      AccompaniedBy:""
    },
      email: "",
      username: "",
      password: "",
      activeScreen: "list",
      events: [],
      selectedEvent: null,
      eventList,
      activeScreen,
      loginstatus,
      accept,
      isbooking,
      slide: ref(1),
     
      autoplay: ref(true),
            options: [
        'Male', 'Female', 'Other']
        };
  },
  computed: {
    computedEventList() {
      return this.eventList;
    },
  },
  methods: {
    async shareCanvas() {
  const canvasElement = document.getElementById('qr-code');
  const dataUrl = canvasElement.toDataURL();
  const blob =  await (await fetch(dataUrl)).blob();
  const filesArray = [
    new File(
      [blob],
      'ticket.png',
      {
        type: blob.type,
        lastModified: new Date().getTime()
      }
    )
  ];
  const shareData = {
    files: filesArray,
  };
  navigator.share(shareData);
},
    Bookticket(yes){
      this.isbooking="true";
      const divElement = document.getElementById('dvdetail');
      divElement.style.display="none";


    },
    onSubmit () {
      
     
        if (this.accept !== true) {
          Notify.create({
            color: 'red-5',
            textColor: 'white',
            icon: 'warning',
            message: 'Please accept the terms first.'
          })
        }
        else {
          const EventsDocRef = collection(db, "booking");
       addDoc(EventsDocRef, this.BookEvent).then((result) => {
       if(result._key.path!=null && result._key.path!=undefined)
       {
     this.BookEvent = {
      AttendeeName:"",
      PhoneNumber:"",
      Personemail:"",
      Gender:"",
      Dietpreference:"",
      AccompaniedBy:""
            };
      this.accept=false;
      new QRious({
          level: "H",
          padding: 25,
          size: 300,
          element:document.getElementById("qr-code"),
          value: result._key.path.segments[1],
        });
           /* this.activeScreen="list";
      this.isbooking="false"; */
      }
          }).catch(error=>{alert(error.message)});
          
             
      
        } 
      },

      onReset () {
        this.BookEvent = {
      AttendeeName:"",
      PhoneNumber:"",
      Personemail:"",
      Gender:"",
      Dietpreference:"",
      AccompaniedBy:""
            };
      this.accept=false;
      },
    handleSignOut()  {
     
        signOut(auth).then(() => {
         // Sign-out successful.
         this.user = null;
         this.loginstatus="false";
         this.isbooking="false";
         this.activeScreen="list";
        alert('User Signed out');
        
        }).catch((error) => {
          // An error happened.
          alert(error.message);
        });
      },
    googleSignIn(){
       const provider=new GoogleAuthProvider();
      
      const auth=getAuth();
     
       signInWithPopup(auth,provider).then((result)=>{
        alert("signed in successfully");
        this.user = result.user.displayName;
       this.loginstatus="true";}).catch(error=>{alert(error.message)});   
      
      
    },
    twitterSignIn(){
      
      const provider =new TwitterAuthProvider();
      const auth=getAuth();
     
      signInWithPopup(auth,provider).then((result)=>{alert("signed in successfully");
       this.user = result.user.displayName;
       this.loginstatus="true";}).catch(error=>{alert(error.message)});
  
    },
    facebookSignIn(){
     const provider=new FacebookAuthProvider();
      
    
     signInWithRedirect(auth,provider).then((result)=>{alert("signed in successfully");
      this.user = result.user.displayName;
      this.loginstatus="true";}).catch(error=>{alert(error.message)});
  },
    onEventClick(objEvent) {
      
      this.selectedEvent = objEvent;
      this.activeScreen = "details:";
    },
    onHomeClick(){
      if(this.activeScreen == "details:")
      {this.activeScreen="list";}
      this.isbooking="false";
    },
    submit(event) {
      // alert(JSON.stringify(event))
      //debugger;
      let email = this.email;
      let password = this.password;
      if (this.mode === "login") {
        signInWithEmailAndPassword(auth, email, password)
          .then((res) => {
            if (
              res != null &&
              res != undefined &&
              res.user.uid != null &&
              res.user.uid != undefined
            )
              alert("Login successful");this.loginstatus="true";
          })
          .catch((err) => {
            alert(err.message);
          });
        onAuthStateChanged(auth, (currentUser) => {
          this.user = currentUser.email;
        });
      } else {
        createUserWithEmailAndPassword(auth, email, password)
          .then((res) => {
            if (
              res != null &&
              res != undefined &&
              res.user.uid != null &&
              res.user.uid != undefined
            )
              alert("User registered successful");this.loginstatus="true";
          })
          .catch((err) => {
            alert(err.message);
          });
      }
    },
    toggleMode(val) {
      this.mode = val;
    },
  },
  mounted() {
    const catsRef = collection(db, "RSVPEvents");
    //const objcategories = [];
    onSnapshot(catsRef, (snapshot) => {
      this.eventList = [];
      snapshot.docs.forEach((doc) => {
        this.eventList.push({ ...doc.data(), id: doc.id });
      });
    });
  },
};
</script>
