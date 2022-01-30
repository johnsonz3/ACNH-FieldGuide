<template>
  <div class="app">
    <nav class="flex items-center justify-between flex-wrap bg-teal-500 p-6">
    <div class="flex items-center flex-shrink-0 text-white mr-6">
      <img class="fill-current h-9 w-8 mr-2" src="../assets/tom_nook.png"> 
      <span class="font-semibold text-xl tracking-tight">Animal Crossing Field Guide</span>
    </div>
    <div class="block lg:hidden">
      <button @click="isOpen = !isOpen" class="flex items-center px-3 py-2 border rounded border-none text-teal-200 border-teal-400 hover:text-white hover:border-white">
        <svg class="fill-current h-6 w-6" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><title>Menu</title>
        <path v-if="!isOpen" d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"/>
        <path v-if="isOpen" fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
        
        </svg>
      </button>
    </div>
    <div class="w-full block flex-grow lg:flex lg:items-center lg:w-auto">
      <div :class="isOpen ? 'block' : 'hidden' " class="text-sm lg:flex-grow">
        <a href="#responsive-header" class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4">
          Bugs
        </a>
        <a href="#responsive-header" class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4">
          Fish
        </a>
      </div>
    </div>
  </nav>
  <p v-if="$fetchState.pending">Fetching Animals...</p>
  <p v-else-if="$fetchState.error">An error occurred :(</p>
  <div v-else class="container my-12 mx-auto px-4 lg:px-12">
    <!-- Search Bar -->
    <div class="flex items-center justify-center m-10">
      <div class="flex border-2 border-teal-500 rounded">
          <input type="text" class="px-4 py-2 w-80" v-model="searchQuery" placeholder="Search...">
          <button class="px-4 text-white bg-teal-500 border-l ">
              Search
          </button>
      </div>
    </div>
    <!-- End Search Bar -->
    <ul>
      <div class="flex flex-wrap justify-around">
      <li v-for="(bug, index) in resultQuery" :key="index">
              <!--Card-->
              <div class="max-w-md mb-10 rounded overflow-hidden shadow-lg bg-teal-500/[0.7] hover:shadow-2xl">
                <img class="w-full" :src="bug.image_uri" alt="Bug">
                <div class="px-6 py-4">
                  <div class="font-bold text-xl mb-2 text-white capitalize">{{ bug.name['name-USen'] }}
                    <button id="button" v-on:click="buttonColor('button')" class="float-right text-sm inline-block rounded-full px-3 py-1 font-semibold text-gray-700 mr-2 mb-2">Add to Collection</button>
                  </div>
                  <p class="text-white text-md outline-10">
                    {{ bug['museum-phrase'] }}
                  </p>
                </div>
                <div class="px-6 pt-4 pb-2">
                  <span class="inline-block bg-yellow-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#Bug</span>
                  <span v-if="insectData != []" class="inline-block bg-green-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#Valid</span>
                  <a class="float-right text-white underline hover:text-blue-700" :href="'https://en.wikipedia.org/wiki/' + insectData[bug[`file-name`]]">Click me to Learn more!</a>
                </div>
              </div>
      </li>
      </div>
    </ul>
  </div>
      <!-- End Container -->
    </div>
</template>

<style lang="scss">
   @import '~assets/scss/variables';
   body {
     background-image: url('assets/background.jpg');
     background-size: 200vh;
  }
  button {
    background-color: #FFFFFF;
  }
</style>

<script>
import { initializeApp } from "firebase/app";
import { getDatabase, ref } from "firebase/database";
const firebaseConfig = {
  apiKey: "AIzaSyBmhBkE2ZSUW4Km5vFzUD4ZdqxA7Hv5Q-o",
  authDomain: "anch-fieldguide.firebaseapp.com",
  databaseURL: "https://anch-fieldguide-default-rtdb.firebaseio.com/",
  projectId: "anch-fieldguide",
  storageBucket: "anch-fieldguide.appspot.com",
  messagingSenderId: "185938110252",
  appId: "1:185938110252:web:b08e7ccbe7da87d82dee89",
  measurementId: "G-Y9BT2JF35X"
};
import {onValue} from "firebase/database";
let count = 0


export default {
  data() {
    return {
      isOpen : true,
      bugs : [],
      searchQuery: "",
      insectData : []
    }
  },
  async fetch() {
    this.bugs = await fetch(
      'http://acnhapi.com/v1/bugs'
    ).then(res => res.json())
  },
  methods : {
    getData: function() {
      const app = initializeApp(firebaseConfig);
      const db = getDatabase();
      const insects = ref(db, 'insects');
      onValue(insects, (snapshot) => {
      this.insectData = snapshot.val();
    });
    },
    buttonColor: function(btn) {
      var property = document.getElementById(btn);
      if (count == 0) {
          property.style.backgroundColor = "#8a8a8a"
          count = 1;        
      } else {
          property.style.backgroundColor = "#FFFFFF"
          count = 0;
      }
    }
  },
  computed: {
    resultQuery: function() {
      var bugs = []
      for (var bug in this.bugs) {
        bugs.push(this.bugs[bug])
      }
      return bugs.filter(bug => bug.name['name-USen'].toLowerCase().includes(this.searchQuery.toLowerCase()));
    },
  },
  beforeMount() {
    this.getData()
  },
  name: 'IndexPage'
}


</script>

