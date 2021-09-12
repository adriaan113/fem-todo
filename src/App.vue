<template>
  <div id="app">
    <div class="container" :class="[night ? bgLight : bgDark]">
      <header :style="{ backgroundImage: 'url(' + bgLightImg + ')' }">
          <div class="title">
            <h1>to do</h1>
            <img :src="nightBtn" alt="" v-if="night" @click="nightMode">
            <img :src="dayBtn" alt="" v-else @click="nightMode">
          </div>
          <input type="text" placeholder="Create a new todo…" :class="[night ? inputLight : bgDark]" v-model="todo.msg" @keyup.enter="todoOnClick">
      </header>
        <toDoList @delete="deleteTodo" :key="componentKey" :color-status="color" :thing="todoToChild" :items-left="counter"/>
    </div>
  </div>
</template>

<script>
import toDoList from './components/toDoList.vue'

export default {
  name: 'App',
  components: {
    toDoList
  },
  props:{
    colorStatus: String,
    thing: Array,
    itemsLeft: Number,
  },
  data: function () {
    return {
      bgLightImg: require('./assets/bg.png'),
      dayBtn: require('./assets/day.svg'),
      nightBtn: require('./assets/night.svg'),

      night: true,
      bgDark: 'bg-dark',
      bgLight: 'bg-light',
      inputLight: 'input-light',

      color: '',

      todo: {
        msg:'',
        completed:false,
      },

      todoToChild: [],

      counter: 0,

      componentKey: 0,
    }
  },
  methods:{
    deleteTodo(idx){
      console.log(idx);
      this.todoToChild.splice(idx,1);
    },
    forceRerender() {
      this.componentKey += 1;
    },
    nightMode(){
      this.night = !this.night;
      
      if(this.night){
          this.color = 'dark';
        }else{
          this.color = 'light';
        }
    },
    todoOnClick(){
      this.todoToChild.push(this.todo);
      this.counter += 1;
      this.todo = {
        msg: '',
        completed: false,
      };
      this.forceRerender();
    }
  },

}
</script>

<style lang="scss">
  @import './global.scss';

  *{
    margin: 0;
    padding: 0;
  }

  #app, .container {
    width: 100%;
    height: 100vh;
    font-family: 'Josefin Sans', sans-serif;
    // background-color: $bg-light;
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
        padding: .5rem 0 .5rem 1rem;
      }
    }
    .bg-light{
      background-color: $bg-light;
    }
    .bg-dark{
      background-color: $bg-dark;
      color: white;
    }

    .input-light{
      background-color: white;
    }

    .menu-dark{
      background-color: $menu-dark;
    }
  }
</style>
