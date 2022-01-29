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
  <div v-else class="container my-12 mx-auto px-4 md:px-12">
    <ul>
      <li v-for="bug in bugs" :key="bug">
        <div class="flex flex-row -mx-1 lg:-mx-4">
              <!--Card 1-->
              <div v-if="bug.id % 3 == 2" class="max-w-sm mb-10 rounded overflow-hidden shadow-lg bg-teal-500/[0.7] cursor-pointer hover:shadow-xl">
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
               <!--Card 2-->
              <div v-if="bug.id % 3 == 1" class="max-w-sm mb-10 rounded overflow-hidden shadow-lg bg-teal-500/[0.7] cursor-pointer hover:shadow-xl">
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

               <!--Card 3-->
              <div v-if="bug.id % 3 == 0" class="max-w-sm mb-10 rounded overflow-hidden shadow-lg bg-teal-500/[0.7] cursor-pointer hover:shadow-xl">
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
          </div>
      </li>
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

const app = initializeApp(firebaseConfig);
import {onValue} from "firebase/database";

const db = getDatabase();
const insects = ref(db, 'insects');
onValue(insects, (snapshot) => {
  const insectData = snapshot.val();
  console.log(insectData);
});

//const dbInsects = query(ref(db, 'insects/'))
// get(child(db, 'insects/')).then((snapshot) => {
//   if (snapshot.exists()) {
//     console.log(snapshot.val());
//   } else {
//     console.log("No data available");
//   }
// }).catch((error) => {
//   console.error(error);
// });

export default {
  data() {
    return {
      isOpen : true,
      bugs : [],
    }
  },
   async fetch() {
      this.bugs = await fetch(
        'http://acnhapi.com/v1/bugs'
      ).then(res => res.json())
    },
  name: 'IndexPage'
}


</script>

