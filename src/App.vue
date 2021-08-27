<template>
  <div id="app">
    <header :style="{ backgroundImage: 'url(' + bgLight + ')' }">
        <div class="title">
          <h1>to do</h1>
          <img :src="nightBtn" alt="" v-if="night" @click="nightMode">
          <img :src="dayBtn" alt="" v-else @click="nightMode">
        </div>
        <input type="text">
    </header>
    <main>
      <toDoList msg="Welcome to Your Vue.js App"/>
    </main>

  </div>
</template>

<script>
import toDoList from './components/toDoList.vue'

export default {
  name: 'App',
  components: {
    toDoList
  },
  data: function () {
    return {
      bgLight: require('./assets/bg.png'),
      dayBtn: require('./assets/day.svg'),
      nightBtn: require('./assets/night.svg'),
      night: true,
    }
  },
  methods:{
    nightMode(){
      this.night = !this.night;
    }
  }
}
</script>

<style lang="scss">
  @import './global.scss';

  *{
    margin: 0;
    padding: 0;
  }

  #app {
    background-color: $bg-light;
    width: 100%;
    height: 100vh;
    font-family: 'Josefin Sans', sans-serif;
    header{
      height: 15vh;
      background-position: center; 
      background-repeat: no-repeat;
      background-size: cover; 
      justify-content: center;
      align-items: center;
      display: flex;
      flex-flow: column nowrap;
      .title{
        display: flex;
        width: 80%;
        justify-content: space-between;
        margin-bottom: 1.5rem;
        max-width: 540px;
        h1{
          text-transform: uppercase;
          color: white;
        }
        img{
          cursor: pointer;
        }
      }
      input{
        width: 80%;
        height: 2rem;
        max-width: 540px;
        border-radius: $br;
        border-style: none;
      }
    }
    
  }
</style>
