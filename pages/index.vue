<template>
  <div class="app">
    <nav class="fixed w-full top-0 flex items-center justify-between flex-wrap bg-teal-500 p-6">
    <div class="flex items-center flex-shrink-0 text-white mr-6">
      <img class="fill-current h-9 w-8 mr-2" src="../assets/tom_nook.png"> 
      <span class="font-semibold text-xl tracking-tight">Animal Crossing Field Guide</span>
    </div>
    <div class="block lg:hidden">
      <button @click="isOpen = !isOpen" class="flex items-center px-3 py-2 border rounded border-none text-teal-200 border-teal-400 hover:text-white hover:border-white">
        <svg class="fill-current h-6 w-6" viewBox="0 0 20 20" xmlns="https://www.w3.org/2000/svg"><title>Menu</title>
        <path v-if="!isOpen" d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"/>
        <path v-if="isOpen" fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
        </svg>
      </button>
    </div>
    <div class="w-full block flex-grow lg:flex lg:items-center lg:w-auto">
      <div :class="isOpen ? 'block' : 'hidden'" class="text-md lg:flex-grow">
        <a id="bug-link" href="#responsive-header" @click="toggleAnimal=true" class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4">
          Bugs  <span class="inline-flex font-bold text-black rounded-full bg-yellow-200 flex items-center justify-center font-mono ml-2" style="height: 20px; width: 20px; font-size: 14px;">{{ bugc }}</span>
        </a>
        <a id="fish-link" href="#responsive-header" @click="toggleAnimal=false" class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4">
          Fish <span class="inline-flex font-bold text-black rounded-full bg-blue-200 flex items-center justify-center font-mono ml-2" style="height: 20px; width: 20px; font-size: 14px;">{{ fishc }}</span>
        </a>
      </div>
    </div>
  </nav>
  <p v-if="$fetchState.pending">Fetching Animals...</p>
  <p v-else-if="$fetchState.error">An error occurred :(</p>
  <div v-else style="margin-top: 10rem;" class="container my-12 mx-auto px-4 lg:px-12">
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
      <li v-for="(animal, index) in resultQuery" :key="index">
              <!--Card-->
              <div class="max-w-md mb-10 rounded overflow-hidden shadow-lg bg-teal-500/[0.7] hover:shadow-2xl">
                <img class="w-full" :src="animal.image_uri" alt="animal">
                <div class="px-6 py-4">
                  <div class="font-bold text-xl mb-2 text-white capitalize">{{ animal.name['name-USen'] }}
                    <div class="circle-container inline-block float-right ">
                      <div class="round">
                        <input type="checkbox" :checked="toggleAnimal ? bugcount[animal['file-name']] : fishcount[animal['file-name']]" @click="validate()" v-bind:id="animal['file-name']"/>
                        <label  :for="animal['file-name']" class="text-sm inline-block rounded-full px-3 py-1 font-semibold text-gray-700 mr-2 mb-2">Add to Collection</label>
                      </div>
                    </div>
                  </div>
                  <p class="text-white text-md outline-10">
                    {{ animal['museum-phrase'] }}
                  </p>
                </div>
                <div class="px-6 pt-4 pb-2">
                  <span :style="tag_color_set" class="inline-block rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{tag}}</span>
                  <span v-if="insectData != []" class="inline-block bg-green-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#Valid</span>                
                  <a v-if="toggleAnimal" class="float-right text-white underline hover:text-blue-700" :href="'https://en.wikipedia.org/wiki/' + insectData[animal[`file-name`]]">Click me to Learn more!</a>
                  <a v-if="!toggleAnimal" class="float-right text-white underline hover:text-blue-700" :href="'https://en.wikipedia.org/wiki/' + fishData[animal[`file-name`]]">Click me to Learn more!</a>
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
  
  .round label {
    background-color: #fff;
    cursor: pointer;
  }

  .round input[type="checkbox"] {
    visibility: hidden;
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
export default {
  data() {
    return {
      isOpen : true,
      bugs : [],
      fishes: [],
      searchQuery: "",
      insectData : [],
      fishData : [],
      toggleAnimal: true,
      tag: '#bug',
      tag_color: {type: String, default:"lightyellow"},
      window: {
            width: 0,
            height: 0
        },
      fishcount: [],
      bugcount: [],
      bugc: 0,
      fishc: 0
    }
  },
  async fetch() {
    this.bugs = await fetch(
      'https://acnhapi.com/v1/bugs'
    ).then(res => res.json());

    this.fishes = await fetch(
      'https://acnhapi.com/v1/fish'
    ).then(res => res.json());
  },
  created() {
      window.addEventListener('resize', this.handleResize);
      this.handleResize();
  },
  destroyed() {
      window.removeEventListener('resize', this.handleResize);
  },
  methods : {
    getData: function() {
      const app = initializeApp(firebaseConfig);
      const db = getDatabase();
      const insects = ref(db, 'insects');
      onValue(insects, (snapshot) => {
      this.insectData = snapshot.val();
    });
      const fish = ref(db, 'fish');
      onValue(fish, (snapshot) => {
      this.fishData = snapshot.val();
    });
    },
    getOccurrence() {
        var count = 0;
        
        if(this.toggleAnimal){
           for(const thing in this.bugcount){
            if(this.bugcount[thing]){
              count = count + 1;
            } 
        }
        }else {
          for(const thing in this.fishcount){
              if(this.fishcount[thing]){
                count = count+1;
              }
        }
        }
        
        return count;
    },
    validate(){
      if(this.toggleAnimal){
        var buggies = Object.values(this.resultQuery);
        for (const bugg in buggies){
          if(document.getElementById(Object.values(buggies[bugg])[1]).checked){
              this.bugcount[bugg] = true;
              document.getElementById(Object.values(buggies[bugg])[1]).nextElementSibling.style['background-color'] = "gray"; 
              document.getElementById(Object.values(buggies[bugg])[1]).nextElementSibling.style['color'] = "white"; 
            }else {
              this.bugcount[bugg] = false;
              document.getElementById(Object.values(buggies[bugg])[1]).nextElementSibling.style['background-color'] = "white";
              document.getElementById(Object.values(buggies[bugg])[1]).nextElementSibling.style['color'] = "black"; 
            }
          }
      }else {
        var fishies = Object.values(this.resultQuery);
        for (const fishh in fishies){
          console.log(Object.values(fishies[fishh])[1]);
          if(document.getElementById(Object.values(fishies[fishh])[1]).checked){
              this.fishcount[fishh] = true;
              document.getElementById(Object.values(fishies[fishh])[1]).nextElementSibling.style['background-color'] = "gray"; 
              document.getElementById(Object.values(fishies[fishh])[1]).nextElementSibling.style['color'] = "white"; 
            }else {
              this.fishcount[fishh] = false;
              document.getElementById(Object.values(fishies[fishh])[1]).nextElementSibling.style['background-color'] = "white";
              document.getElementById(Object.values(fishies[fishh])[1]).nextElementSibling.style['color'] = "black"; 
            }
          }
      }
      if(this.tag == '#bug') {
          this.bugc = this.getOccurrence("bug");
        } else {
          this.fishc =  this.getOccurrence("fish");
        }
    },
    handleResize() {
        this.window.width = window.innerWidth;
        this.window.height = window.innerHeight;

        if(window.innerWidth >= 1024) {
          this.isOpen = true;
        }
      }
    },
  computed: {
    resultQuery: function() {
      var bugs = []
      var fishes = []
      if(this.toggleAnimal){
        this.tag = '#bug'
        this.tag_color = 'lightyellow'
        for (var bug in this.bugs) {
          bugs.push(this.bugs[bug])
        }
        this.data_bugs = bugs;
        return bugs.filter(bug => bug.name['name-USen'].toLowerCase().includes(this.searchQuery.toLowerCase()));
      }else {
        this.tag = '#fish'
        this.tag_color = 'lightblue'
        for (var fish in this.fishes) {
          fishes.push(this.fishes[fish])
        }
        this.data_fishes = fishes;
        return fishes.filter(fish => fish.name['name-USen'].toLowerCase().includes(this.searchQuery.toLowerCase()));
      }
    },
    tag_color_set() {
      return {
        "background-color" : this.tag_color
      };
    }
  },
  beforeMount() {
    this.getData();
  },
  name: 'IndexPage'
}


</script>

