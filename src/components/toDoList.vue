<template>
  <div class="list">
    <h1 class="color-status">{{colorStatus}}{{thing}}{{itemsLeft}}</h1>
    <ul class="list-cotainer">
      <li class="item" :class="[darkLight ? dark : light]" v-for="(item,index) in alfie" :key="item.id" :counter="itemsLeft">
          <input type="checkbox" name="" id="">
          <p>{{item}} - {{index}}</p>
          <span @click="deleteTodo(item,index)">x</span>
      </li>
      <li class="count-completed" :class="[darkLight ? dark : light]">
        <p> {{counter}} items left</p>
        <p> clear completed</p>
      </li>
    </ul>
    <ul class="control">
      <li class="select-items" :class="[darkLight ? dark : light]">all</li>
      <li class="select-items" :class="[darkLight ? dark : light]">active</li>
      <li class="select-items" :class="[darkLight ? dark : light]">completed</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'toDoList',
  props: {
    colorStatus: String,
    thing: Array,
    itemsLeft: Number,
  },
  data: function(){
    return {
      night: true,
      dark: 'dark',
      light: 'light',
      darkLight: '',
      alfie: this.thing, 
      isActive: [],

      counter: this.itemsLeft,
    }
  },  
  computed:{
    satan(){
      return this.thing.map((x)=> x);  
    },
  },
  methods:{
    setThemeColor(){
      if(this.colorStatus === 'dark'){
        this.darkLight = false;
      }else if(this.colorStatus === 'light'){
        this.darkLight = true;
      }
    },
    deleteTodo(name,idx){
      // console.log(this.alfie);
      //console.log(idx);
      //console.log(this.alfie.indexOf(name));      
      if ( this.alfie.indexOf(name) === idx) { 
        return this.alfie.splice(idx, 1); 
      }
    },
  },
  updated(){
    this.setThemeColor();
  },
  mounted(){
    this.setThemeColor();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss"  scoped>
@import '../global.scss';

.list{
  width: 80%;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  margin: 0 auto;
  ul{
    width: 100%;
    background-color: white;
    display: flex;
    flex-flow: column nowrap;
    align-items: center;
    padding: 0;
    list-style: none;
    border-radius: $br;
    margin-bottom: 1rem;
    .item{
      width: 100%;
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      position: relative;
      height: 2rem;
      padding: .5rem 0;
      border-bottom: 1px solid $bg-dark;
      align-items: center;
      &:last-child{
        border-bottom: none;
      }
      input,p,span{
        margin: 0 1rem;
      }
      span{
        cursor: pointer;
      }
      p{
        position: absolute;
        left: 30px;
      }
    }
    .count-completed{
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      width: 100%;
      position: relative;
      height: 2rem;
      padding: .5rem 0;
      align-items: center;
      p{
        position: static;
        left: auto;
        margin: 0 1rem;
      }
      
    }
  }
  .control{
    width: 100%;
    background-color: white;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    justify-content: center;
    padding: 0;
    border-radius: $br;
    .select-items{
      width: 100%;
      display: flex;
      flex-flow: row nowrap;
      justify-content: center;
      height: 2rem;
      padding: .5rem 0;
      align-items: center;
    }
  }

  .dark{
    background-color: $menu-dark;
    color: white;
  }

  .light{
    background-color: white;
  }

  .color-status{
    display: none;
  }
}

</style>
