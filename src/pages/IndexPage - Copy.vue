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
     this is active  {{ selectedEvent }}
      <RegisterUser :name="activeScreen"></RegisterUser>
      <q-card  class="my-card"  rounded>
      <img src="https://cdn.quasar.dev/img/parallax2.jpg">
      
      <q-list style="display: flex; align-items:baseline;">
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

          <q-item-section>
            <q-item-label>Book your ticket</q-item-label>
          
          </q-item-section>
        </q-item>
      </q-list>
    </q-card>
    
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
//import { defineComponent } from "vue";
//import { useQuasar } from 'quasar'
import RegisterUser from 'src/components/RegisterUser.vue';
export default ({
  name: "IndexPage",
  components:{
    RegisterUser,
  },
  data(){
    return {
      activeScreen:'list',
      events:[],
      selectedEvent:null,
      lorem:
         "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
     };
    }
    ,
    methods:{
    onEventClick(event){
      debugger;
      this.selectedEvent=event;
      this.activeScreen='details'

    }
  }
  /* ,
  mounted() {
    this.events= response;
  },
  beforeCreate(){

  } */
  });
 
  
//   setup() {
  
//     const $q = useQuasar()

// // calling here; equivalent to when component is created
// $q.dark.set(true)
//     return {
//       lorem:
//         "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
//     };
//   },
//);
</script>