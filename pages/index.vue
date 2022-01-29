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
          Docs
        </a>
        <a href="#responsive-header" class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4">
          Examples
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
      <li v-for="bug in resultQuery" :key="bug">
              <!--Card 1-->
              <div class="max-w-md mb-10 rounded overflow-hidden shadow-lg bg-teal-500/[0.7] cursor-pointer hover:shadow-xl">
                <img class="w-full" :src="bug.image_uri" alt="Bug">
                <div class="px-6 py-4">
                  <div class="font-bold text-xl mb-2 text-white capitalize underline">{{ bug.name['name-USen'] }}</div>
                  <p class="text-white text-md outline-10">
                    {{ bug['museum-phrase'] }}
                  </p>
                </div>
                <div class="px-6 pt-4 pb-2">
                  <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#Bug</span>
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
</style>

<script>
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
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

const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
export default {
  data() {
    return {
      isOpen : true,
      bugs : [],
      searchQuery: ""
    }
  },
  async fetch() {
    this.bugs = await fetch(
      'http://acnhapi.com/v1/bugs'
    ).then(res => res.json())
  },
  computed: {
    resultQuery: function() {
      var bugs = []
      for (var bug in this.bugs) {
        bugs.push(this.bugs[bug])
      }
      return bugs.filter(bug => bug.name['name-USen'].toLowerCase().includes(this.searchQuery.toLowerCase()));
 
    }
  },
  name: 'IndexPage'
}


</script>

