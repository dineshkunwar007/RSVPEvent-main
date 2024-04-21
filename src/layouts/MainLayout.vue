<template>
  <q-layout view="lHh Lpr lFf"   class="shadow-2 rounded-borders">
    <div v-if="activeScreen == 'list'||activeScreen == 'details:' || loginstatus!='true'">
    <q-header bordered class="bg-grey-3 text-primary" elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer" class="q-mr-sm" style="color: red;"
        />
        <q-avatar rounded >
            <img src="icons/mymanu.png">
          </q-avatar>
        <q-toolbar-title style="color: red;">
          Mymanu Events
        </q-toolbar-title>

       <!--  <div>Mymanu v{{ $q.version }}</div> -->
      </q-toolbar>
    </q-header></div>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Essential Links
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
        <q-toggle
      :label="`Dark theme enabled: ${blueModel}`"
      v-model="blueModel" val="true" @update:model-value="onToggleChange"  />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
    <!-- <q-footer >
      <q-tabs v-model="tab"
        no-caps
        outside-arrows
        mobile-arrows
        class="bg-primary text-white shadow-2"
      >
        <q-tab name="images"><img src="icons/50.png"></q-tab>
        <q-tab name="videos"> <img src="icons/panel.png"></q-tab>
        <q-tab name="articles"><img src="icons/profile.png"></q-tab>
      </q-tabs>
    </q-footer> -->
  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import { event, useQuasar,Dark  } from 'quasar'
const linksList = [
  
  {
    title: 'contact us at:',
    caption: 'support@mymanu.co.uk',
    icon: 'email',
    link: 'mailto:support@mymanu.co.uk?subject= Event Query'
   /*  <a href="mailto:email@example.com?subject=Mail from our Website">Send Email</a> */
  },
  {
    title: 'User profile',
    caption: 'Change user details here',
    icon: 'person',
    link: ''
  }/* ,
  {
    title: 'Mymanu',
    caption: 'Apolo',
    icon: 'rss_feed',
    link: 'https://mymanu.com/products/mymanu-apollo'
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev'
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev'
  } */
]

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup () {
    const leftDrawerOpen = ref(false)
    
    return {
      blueModel: ref(true),
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  },
  methods:{
    onToggleChange(newValue) {    
      newValue? Dark.set(true):Dark.set(false)   

    }
  }
})
</script>
