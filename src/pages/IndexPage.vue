<template>

  <q-page class="flex">
    <div>
  <div v-if="user">Signed In User: {{ user }}</div>
  <div v-if="loginstatus=='true'">
        <q-btn @click="handleSignOut()" color="red" style=" border: 2px solid #f44336; position: absolute;right: 10px;top:0px" icon="logout" >Sign Out</q-btn>
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
            <q-btn type="submit" unelevated size="lg" color="red-4" class="full-width text-white">{{
          mode === "login" ? "Login" : "Register"
        }}</q-btn>
           
          </q-card-actions>
            </q-form>
            
          </q-card-section>
          
          <q-card-section>
            <div class="text-center q-pa-md q-gutter-md">
             
              <q-btn class="transparent" @click="googleSignIn()" >
              
                <img src="icons/icons8-google-50.png"/>           </q-btn>
              <q-btn @click="twitterSignIn()" >
                <img src="icons/icons8-twitter-50.png"/>
              </q-btn>
              <q-btn @click="facebookSignIn()">
                <img src="icons/icons8-facebook-50.png"/>
              </q-btn>
            </div>
          </q-card-section>
          
          <q-card-section class="text-center q-pa-sm">
            <!-- <p class="text-grey-6">Forgot your password?</p> -->
            <div class="text-red-6" style="-webkit-text-fill-color:grey;text-decoration-color: red;" @click="toggleMode(mode === 'login' ? 'register' : 'login')">
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
     <!--  <RegisterUser :name="activeScreen"></RegisterUser> -->
      <div class="q-pa-md row items-start q-gutter-md">
       <q-card class="my-card">
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
    </q-carousel>
      <q-card-section class="q-pt-none">
          {{ selectedEvent.eventTitle }}
        </q-card-section>
        <q-card-section class="q-pt-none">
          {{ selectedEvent.eventSummary }}
        </q-card-section>
        <q-card-section class="q-pt-none">
          {{ "Contact person: "+selectedEvent.contactPerson }}
        </q-card-section>
        <q-card-section class="q-pt-none">
          {{ "Contact number: "+selectedEvent.contactPhone }}
        </q-card-section>
        <q-card-section class="q-pt-none">
          {{ "Tickets remaining: "+selectedEvent.totalseat }}
        </q-card-section>
       
        <q-list style="align-items: baseline">
        
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
  
  <div id="dvqr" style="margin:50px;display: none; width: 50%">
    <canvas width="50%" id="qr-code"></canvas>
    <div style="float:left;">
    
        <div>Ticket booked successfully:</div>
        <q-btn square color="primary" flat class="q-ml-sm" label="share" @click="shareCanvas()">               
                  <q-icon name="share"  />               
              </q-btn>
       <q-btn square color="primary" flat class="q-ml-sm" label="close" @click="CloseCanvas()">                
                  <q-icon name="close" />               
              </q-btn>
      </div>
       
        
        
  </div>
  
  <div id="dvbookedevents"
      class="q-pa-md row items-start q-gutter-md"
      v-if="activeScreen == 'bookedevents'"
      v-for="obj,index in computedbookedList"
      :key="obj.id"
    >
    <q-card  @click="draftcanvasticket(index,obj)" aria-label="please click to view QR tickets" class="my-card">    
      <canvas width="50%"  id="qr"></canvas> 
    <q-card-section>
      <div class="text-subtitle2"> This a ticket for {{ obj.AttendeeName}} with email {{ obj.Personemail}}</div>
      <div class="text-h6"> Event: {{ obj.EventTitle }}</div>
          
        </q-card-section>
  </q-card>
      <!-- <q-card class="my-card" @click="onEventClick(objEvent)">
        <img :src="objEvent.eventImg" />

        <q-card-section>
          <div class="text-h6">{{ objEvent.eventTitle }}</div>
          <div class="text-subtitle2">{{ objEvent.createdby }}</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          {{ objEvent.eventSummary }}
        </q-card-section>
      </q-card> -->
      <!-- EVENT Details start-->

      <!-- EVENT Details end-->
    </div>
   <div v-if="activeScreen == 'details:' ">
   
       <q-footer bordered class="bg-grey-3 text-primary" elevated  >
      <q-tabs
   
        v-model="tab"
      >
        <q-tab @click="onHomeClick" name="images"><img src="icons/icons8-home-64.png" /></q-tab>
        <q-tab name="videos"> <img src="icons/icons8-panel-48.png" /></q-tab>
        <q-tab  @click="onbookedEvents" name="articles"><img src="icons/icons8-user-64.png" /></q-tab>
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
    const bookedList = ref([]);
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
    
    $q.dark.set(false);
    return {
      mode: ref("login"),
      BookEvent:{
      AttendeeName:"",
      EventId:"",
      EventTitle:"",
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
      eventList,bookedList,
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
      return this.eventList
     
    },
    computedbookedList(){
      return this.bookedList
    },
  },
  methods: {
    CloseCanvas() { 
      const Element = document.getElementById('dvqr');
      Element.style.display="none"; 
    this.activeScreen="list";
      this.isbooking="false";   },
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
       {   Notify.create({
            color: 'green-4',
            textColor: 'white',
            icon: 'cloud_done',
            message: 'Ticket booked successfully for event:'+this.selectedEvent.eventTitle
          })
        const Element = document.getElementById('dvqr');
      Element.style.display="block";         //dvqr
    
      this.accept=false;
      new QRious({
          level: "H",
          padding: 25,
          size: 300,
          element:document.getElementById("qr-code"),
          value: result._key.path.segments[1]+'@'+ this.BookEvent.AttendeeName+'@'
          +this.selectedEvent.eventTitle+'@'+this.selectedEvent.id });
        this.BookEvent = {
      AttendeeName:"",
      EventId:"",
      EventTitle:"",
      PhoneNumber:"",
      Personemail:"",
      Gender:"",
      Dietpreference:"",
      AccompaniedBy:""
            };
      this.isbooking="false"; 
      }
          }).catch(error=>{alert(error.message)});
                     
      
        } 
      },

      onReset () {
        this.BookEvent = {
      AttendeeName:"",
      EventId:"",
      EventTitle:"",
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
         const Element = document.getElementById('dvqr');
      Element.style.display="none";    
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
      this.BookEvent.EventId= this.selectedEvent.id;
      this.BookEvent.EventTitle= this.selectedEvent.eventTitle;
      
    },
    draftcanvasticket(index,obj){
      alert("drfat");
      document.getElementById("qr").id = "qr"+index;
        new QRious({
          level: "H",
          padding: 25,
          size: 300,
          element:document.getElementById("qr"+index),
          value: obj.id+'@'+ obj.AttendeeName+'@'
          +this.selectedEvent.eventTitle+'@'+this.selectedEvent.id }); 
        },
    onbookedEvents()
    { this.activeScreen="bookedevents";

      alert(JSON.stringify(this.bookedList))
    },
    onHomeClick(){
      if(this.activeScreen == "details:")
      {this.activeScreen="list";}
      this.isbooking="false";
      const Element = document.getElementById('dvqr');
      Element.style.display="none"; 
      this.BookEvent = {
      AttendeeName:"",
      PhoneNumber:"",
      Personemail:"",
      Gender:"",
      Dietpreference:"",
      AccompaniedBy:""
            };
    },
    submit(event) {
      // alert(JSON.stringify(event))
      //tst
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
    const appliedbookings = collection(db, "booking");
    onSnapshot(catsRef, (snapshot) => {
      this.eventList = [];
      snapshot.docs.forEach((doc) => {
        this.eventList.push({ ...doc.data(), id: doc.id });
      });
    });
    onSnapshot(appliedbookings, (snapshot) => {
      this.bookedList = [];
      snapshot.docs.forEach((doc) => {
        this.bookedList.push({ ...doc.data(), id: doc.id });
        
      });
    }); 
   // alert(JSON.stringify(this.bookedList));
  },
};
</script>
