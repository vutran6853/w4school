<template>
  <div id="navbarItems" class="navbarItems-container" v-on:scroll.passive='handleScroll' v-bind:class='state.navbarItemsClassStatus'>
    <div class="navbarItems-group-items-1 " v-if='navbarItemGroup1.isTrue'>
      <ul>
        <li v-for='(value) in navbarItemGroup1.items' 
          v-bind:key='value.id'
          v-on:click='handleToggleModel'
          >{{ value.name }}</li>
      </ul>

      <!-- <div>
        <p>toggleIron</p>
        <p>earth</p>
        <p>search</p>
      </div> -->
    </div>

    <div class="navbarItemGroup1-model-container p-1" v-if='state.isModel'>
      <div class="container w-25 mr-1">
        <h3 class="mb-1">HTML and CSS</h3>
        <p v-on:click='handleRouteTo2("html")'>Learn HTML</p>
        <p v-on:click='handleRouteTo2("css")'>Learn CSS</p>
      </div>

      <div class="container w-25">
        <h3 class="mb-1  mr-1">JavaScript</h3>
        <p v-on:click='handleRouteTo2("javascript")'>Learn JavaScript</p>
      </div>
    </div>

    <div class="navbarItems-group-items-2" v-if='navbarItemGroup2.isTrue'>
      <ul>
        <li v-for='(value) in navbarItemGroup2.items' 
          v-bind:key='value.id'
          v-bind:class='value.isHover ? "active" : "" '
          v-on:click='handleRouteTo(value)'
          >{{ value.name }}</li>
      </ul>

      <!-- <div>
        <p>toggleIron</p>
        <p>earth</p>
        <p>search</p>
      </div> -->
    </div>


    <div class="navvbarItems-group-items-0 " v-if='navbarItemGroup1.items[0].isShowInfo'>
      <div>
        <h3>HTML and CSS</h3>
        <p>Learn HTML</p>
        <p>Learn CSS</p>
        <p>Learn Bootstrap</p>
        <p>Learn w6.CSS</p>
        <p>Learn Colors</p>
        <p>Learn Icons</p>
        <p>Learn Graphices</p>
        <p>Learn SVG</p>
        <p>Learn Canvas</p>
        <p>Learn How To</p>
        <p>Learn Sass</p>
      </div>

      <div>
        <h3>JavaScript</h3>
        <p>Learn JavaScript</p>
        <p>Learn jQuery</p>
        <p>Learn React</p>
        <p>Learn AngularJS</p>
        <p>Learn JSON</p>
        <p>Learn AJAX</p>
        <p>Learn w6.JS</p>
      </div>


      <div>
        <h3>Programming</h3>
        <p>Learn Python</p>
        <p>Learn Java</p>
        <p>Learn C++</p>
        <p>Learn C#</p>
        <p>Learn Machine Learning</p>
      </div>

    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { getter, action } from '../../store/store'

type TnavbarItemGroup1 = {
  id: number,
  name: string,
  isHover: boolean,
  isShowInfo: boolean
}

type TnavbarItemGroup2 = {
  id: number,
  name: string,
  isHover: boolean,
  isShowInfo: boolean
}

interface INavbarItemsState {
  state: {
    navbarItemGroup1: {
      isTrue: boolean,
      items: TnavbarItemGroup1[]
    },
    navbarItemGroup2: {
      isTrue: boolean,
      items: TnavbarItemGroup2[]
    }
  }
}

const NavbarItems = Vue.extend({
  name: 'NavbarItems',
  data() {
    return {
      state: {
        navbarItemsClassStatus: 'navbarItems-group-items-2',
        isModel: false
      }
    }
  },
  computed: {
    navbarItemGroup1: {
      get() {
        return this.$store.getters[getter.getNavbarItemGroup1]
      }
    },
    navbarItemGroup2: {
      get() {
        return this.$store.getters[getter.getNavbarItemGroup2]
      }
    }
  },
  methods: {

    /** 
     * * Will route to right tab and update vuex
    **/
    handleRouteTo(valueObj: TnavbarItemGroup2) {
      this.$store.dispatch(action.setNavbarItemsBySelected, valueObj.id)
      this.$router.push({
        path: `/${valueObj.name.toLowerCase()}`
      })
    },
    handleRouteTo2(urlPath: string) {
      this.$router.push({
        path: `/${urlPath}`
      })
    },
    handleScroll() {
      if (window.scrollY < 100) {
        // this.state.navbarItemsClassStatus = 'navbarItems-group-items-2'
      } else {
        // this.state.navbarItemsClassStatus = 'navbarItems-container-fixed'
        // this.handleRemoveScrollEffect()
      }
      return null
    },
    handleToggleModel() {
      this.state.isModel = !this.state.isModel
    }
  }
})

export default NavbarItems
</script>

<style>
.navbarItems-container {
  position: relative; 
}

.navbarItems-container-fixed {
  position: fixed;
  left: 14rem;
  width: 100%;
  z-index: 1;
}
.navbarItems-group-items-1 {
  height: 60px;
  background-color: green;
  display: flex;
  justify-content: space-between;
}

.navbarItems-group-items-2 {
  height: 60px;
  background-color: #5f5f5f;
  display: flex;
  justify-content: space-between;
}

.navbarItems-group-items-1 > ul {
  /* background-color: lightseagreen; */
  display: flex;
  flex-wrap: wrap;
  /* width: 50%; */
  font-size: 24px;
  list-style: none;
  justify-content: space-around;
  align-items: center;
  color: whitesmoke;

}

.navbarItems-group-items-1 > ul > li {
  /* background-color: royalblue; */
    height: 100%;
    width: 10rem;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

.navbarItems-group-items-1 > ul > li:hover {
  background-color: whitesmoke;
  color: black;
}

.navbarItems-group-items-2 > ul {
  display: flex;
  flex-wrap: wrap;
  font-size: 24px;
  list-style: none;
  justify-content: space-around;
  align-items: center;
  color: whitesmoke;
}

.navbarItems-group-items-2 > ul > li {
  background-color: #4CAF50;
  height: 100%;
  width: 10rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.navbarItems-group-items-2 > ul > li:not(.active)  {
  background-color: #5f5f5f;
}

.navbarItems-group-items-2 > ul > li:hover {
  background-color: black;
}

.navbarItemGroup1-model-container {
  background-color: #f1f1f1;
  height: 15rem;
  width: 100%;
  position: absolute;
  top: 3.75rem;
  display: flex;

}

.navbarItemGroup1-model-container > .container > p {
  padding-top: 0.3rem;
  padding-bottom: 0.3rem;
}
.navbarItemGroup1-model-container > .container > p:hover {
  background-color: #5f5f5f;
  cursor: pointer;
}

@media only screen and (max-device-width: 480px) {
  #navbarItems > div.navbarItems-group-items-1 {
    width: 100%;
  }

  #navbarItems > div.navbarItems-group-items-1 > ul {
    font-size: 1rem;
    flex-direction: column;
  }
  #navbarItems > div.navbarItems-group-items-2 > ul {
    font-size: 1rem;
    flex-direction: column;
  }

  #navbarItems > div.navbarItems-group-items-1 > ul > li {
    /* width: 6.5rem; */
    width: 7.54375rem;
  }
    #navbarItems > div.navbarItems-group-items-2 > ul > li {
    /* width: 6.5rem; */
    width: 7.54375rem;
  }
}

</style>
