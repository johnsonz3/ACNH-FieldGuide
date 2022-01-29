<template>
  <div class="app">
    <nav class="flex items-center justify-between flex-wrap bg-teal-500 p-6">
    <div class="flex items-center flex-shrink-0 text-white mr-6">
      <img class="fill-current h-9 w-8 mr-2" src="../assets/tom_nook.png"> 
      <span class="font-semibold text-xl tracking-tight">Animal Crossing Field Guide</span>
    </div>
    <div class="block lg:hidden">
      <button @click="isOpen = !isOpen" class="flex items-center px-3 py-2 border rounded text-teal-200 border-teal-400 hover:text-white hover:border-white">
        <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><title>Menu</title><path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"/></svg>
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
export default {
  data() {
    return {
      isOpen : false,
    }
  },
  name: 'IndexPage'
}
</script>
