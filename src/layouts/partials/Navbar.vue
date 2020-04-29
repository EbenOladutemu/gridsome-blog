<template>
  <div class="fixed inset-0 h-16 bg-black">
    <nav 
    class="flex navbar navbar-expand flex-column flex-md-row items-center justify-between flex-wrap container mx-auto px-4 sm:px-0 py-4 pt-15 transition-all transition-500" 
    v-bind:class="{
      'opacity-100': !disableScroll && scrollPosition > headerHeight, 
      'opacity-0': !disableScroll && scrollPosition < headerHeight
    }">
      <div class=" flex-grow flex justify-between items-center w-auto">
        <div class="flex items-center flex-shrink-0 text-white mr-6">
          <!-- <font-awesome :icon="['fas', 'ghost']" class="mr-3"></font-awesome> -->
          <img src="./../../assets/img/latest-9ja.png" style="width: 35px; margin-right: 10px" alt="Latest 9ja">
          <span class="font-semibold text-xl tracking-tight">{{ $static.metadata.siteName }}</span>
        </div>
        <div class="text-sm flex-grow uppercase navbar-nav-scroll d-none d-sm-block d-sm-none d-md-block d-md-none d-lg-block hide">
          <ul 
          class="list-none flex justify-left text-gray-300 uppercase transition-all transition-500 navbar-nav mr-auto bd-navbar flexrow">
            <li
              :key="element.name"
              v-for="(element,index) in $static.metadata.navigation"
              class="hover:text-white nav-item"
              v-bind:class="{'mr-4' : index != Object.keys($static.metadata.navigation).length - 1}"
            >
              <a
                :href="element.link"
                v-if="element.external"
                target="_blank"
                rel="noopener noreferrer"
                class="animated-link"
              >{{ element.name }}</a>
              <g-link v-else :to="element.link" class="animated-link">{{element.name}}</g-link>
            </li>
          </ul>
        </div>
        
        <div class="inline-block text-gray-400">
          <ul class="list-none flex justify-center md:justify-end">
            <li class="mr-0 sm:mr-6">
              <theme-switcher v-on="$listeners" :theme="theme"/>
            </li>
            <li
              :key="element.name"
              v-for="(element,index) in $static.metadata.social"
              class="hover:text-white hidden sm:block"
              v-bind:class="{'mr-6' : index != Object.keys($static.metadata.social).length - 1}"
            >
              <span class="text-sm">
                <a :href="element.link" target="_blank" rel="noopener noreferrer">
                  <font-awesome :icon="['fab', element.icon]" />
                </a>
              </span>
            </li>
          </ul>
        </div>

      </div>
    </nav>
    <div class="text-sm flex-grow uppercase navbar-nav-scroll nav-adj bg-black d-block d-sm-none d-none d-sm-block d-md-none hide-2">
      <ul 
      class="list-none flex justify-center text-gray-300 uppercase transition-all transition-500 navbar-nav mr-auto bd-navbar flexrow flex-dir-row">
        <li
          :key="element.name"
          v-for="(element,index) in $static.metadata.navigation"
          class="hover:text-white nav-item"
          v-bind:class="{'mr-4' : index != Object.keys($static.metadata.navigation).length - 1}"
        >
          <a
            :href="element.link"
            v-if="element.external"
            target="_blank"
            rel="noopener noreferrer"
            class="animated-link"
          >{{ element.name }}</a>
          <g-link v-else :to="element.link" class="animated-link">{{element.name}}</g-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
/*
 * I'm a lazy guy, so i used this script
 * https://codepen.io/ninaregli/pen/OjeMLP
 * to calculate the current scroll position
 *
 * Will be used to add/remove the additional
 * css classes to show the sticky navbar
 */

import ThemeSwitcher from '~/components/ThemeSwitcher'

export default {
  components : {
    ThemeSwitcher
  },
  props: {
    disableScroll: {
      type: Boolean,
      default: false
    },
    theme: {
      type: String
    }
  },
  data: function() {
    return {
      scrollPosition: null,
      headerHeight: 0
    };
  },

  methods: {
    updateScroll() {
      this.scrollPosition = window.scrollY;
    },
    setHeaderHeight(height) {
      this.headerHeight = height;
    }
  },

  mounted() {
    if( !this.disableScroll ) {
      var height = document.getElementById("header").clientHeight;
      this.setHeaderHeight(height);
      window.addEventListener("scroll", this.updateScroll);
    }
  }
};
</script>

<static-query>
query {
  metadata {
    siteName
    navigation : headerNavigation {
      name
      link
      external
    }
    social {
      icon
      link
    }
  }
}
</static-query>

<style scoped>
  @media (max-width: 900px){
    .navbar-nav-scroll {
      max-width: 100%;
      height: 1.5rem;
      /* margin-top: .25rem; */
      overflow: scroll;
    }
    .text-center{
      text-align: center;
    }
    .hide{
      display: none;
    }
  }
  @media (min-width: 901px){
    .hide-2{
      display: none;
    }
  }
  .nav-item{
    flex: none;
  }
  @media (min-width: 640px){
    .sm\:px-0 {
      padding-left: 2.5rem!important;
      padding-right: 2.5rem!important; 
    }
  }
  .nav-adj{
    position: relative;
    bottom: 25px!important;
    margin-top: 13px!important;
    height: 35px!important;
  }
  .flex-dir-row{
    flex-direction: row!important;
  }
  .pt-15{
    padding-top: 15px!important;
  }
</style>
