<template>
  <div class="list">
    <h1 class="color-status">{{colorStatus}}{{thing}}{{itemsLeft}}</h1>
    <ul class="list-cotainer" >
      <li class="item" 
          :class="toggleDarkLight" 
          v-for="(item,index) in alfie" 
          :key="item.id" 
          :counter="itemsLeft"
          draggable
          @dragover="(e) => onDragOver(item, index, e)" 
          @dragend="(e) => finishDrag(item, index, e)"
          @dragstart="(e) => startDrag(item, index, e)"
          >
          <label class="checkbox-container">
            <input type="checkbox" name="" id="checkbox"  v-model="item.completed" @change="addCompleted">
            <span class="checkmark"></span>
          </label>
          
          <p :class="{completed: item.completed}">{{item.msg}}</p>
          <!-- <span @click="deleteTodo(item,index)">x</span> -->
          <img @click="deleteTodo(item,index)" :src="cross" alt="">
      </li>
      <li class="count-completed" :class="toggleDarkLight">
        <p v-if="counter != 0"> {{counter}} items left</p>
        <p v-else>All done!</p>
        <p class="clear-completed" @click="clearCompleted"> clear completed</p>
      </li>
    </ul>
    <ul class="control">
      <li class="select-items" 
          :class="[toggleDarkLight,filterAll]"
          @click="showAll"
          >
          All
      </li>
      <li class="select-items" 
          :class="[toggleDarkLight,filterActive]"
          @click="showActive"
          >
          Active
      </li>
      <li class="select-items" 
          :class="[toggleDarkLight,filterCompleted]" 
          @click="showCompleted">
          Completed
      </li>
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

      result: [],

      alfie: this.thing.map(x => ({...x})),
  
      completed:[],

      isActive: {
        all: false,
        active: false,
        completed: false,
      },
      

      done:'done',

      over: {},
      startLoc: 0,
      dragging: false,
      dragFrom: {},

      cross: require('../assets/cross.svg'),
    }
  },  
  computed:{
    counter(){
      return this.alfie.length;
    },
    toggleDarkLight(){
      return this.darkLight ? this.dark : this.light;
    },
    filterAll(){
        return{
          active: this.isActive.all     
        }  
    },
     filterActive(){
        return{
          active: this.isActive.active     
        }  
    },
     filterCompleted(){
        return{
          active: this.isActive.completed     
        }  
    }
  },
  methods:{  
    
    checkComplete(){
      for(let item of this.thing){
        if(item.completed){
            this.completed.push(item);
        }
      }  
    },
    
    setThemeColor(){
      if(this.colorStatus === 'dark'){
        this.darkLight = false;
      }else if(this.colorStatus === 'light'){
        this.darkLight = true;
      }
    },
    deleteTodo(name,idx){    
      if ( this.alfie.indexOf(name) === idx) { 
        this.alfie.splice(idx, 1);
        this.$emit('delete',idx); 
      }
      
    },
    addCompleted(){
      this.completed= this.alfie.filter((item)=>item.completed);
      this.$emit('complete',this.completed);
      // return this.completed;
    },
    clearCompleted(){
      for(let i = this.alfie.length -1; i>=0;--i){
        if(this.alfie[i].completed){
          this.alfie.splice(i,1);
        }
      }
    },
    showCompleted(){
      if(this.completed.length===0){
        return
      }else{
        this.alfie.splice(this.alfie,this.alfie.length);
        this.$emit('complete',this.completed);
        
        for(let i=0;i<this.thing.length;i++){
          if(this.thing[i].completed===true){
            this.alfie.push(this.thing[i]);
          }
        }
        this.isActive.all=false;
        this.isActive.active=false;
        this.isActive.completed=true;
      } 
    },

    showActive(){
      this.alfie.splice(this.alfie,this.alfie.length);
      for(let i=0;i<this.thing.length;i++){
        if(this.thing[i].completed===false){
          this.alfie.push(this.thing[i]);
        }
      }
      this.isActive.all=false;
      this.isActive.active=true;
      this.isActive.completed=false;
    },

    showAll(){
      this.alfie.splice(this.alfie,this.alfie.length);
      for(let i=0;i<this.thing.length;i++){
          this.alfie.push(this.thing[i]);
        }
        this.isActive.all=true;
        this.isActive.active=false;
        this.isActive.completed=false;
    },

    startDrag(item, i, e) {
      this.startLoc = e.clientY;
      this.dragging = true;
      this.dragFrom = item;
      console.log(this.dragFrom);
    },
    finishDrag(item, pos) {
      this.alfie.splice(pos, 1)
      this.alfie.splice(this.over.pos, 0, item);
      this.over = {}
    },
    
    onDragOver(item, pos, e) {
      const dir = (this.startLoc < e.clientY) ? 'down': 'up';
      this.over = { item, pos, dir };      
    },
  },
  updated(){
    this.setThemeColor(); 
    this.checkComplete(); 
  },
  mounted(){
    this.setThemeColor();
  },
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
  margin: -1rem auto 0;
  font-family: 'Roboto', sans-serif;
  p{
    font-size: .8rem;
  }
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
      border-bottom: 1px solid $border-color-light;
      align-items: center;
      cursor: grab;
      &:last-child{
        border-bottom: none;
      }
      input,p,img{
        margin: 0 1rem;
      }
      img{
        cursor: pointer;
      }
      p{
        position: absolute;
        left: 30px;
      }
      .completed{
        color: grey;
        text-decoration: line-through;
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
      border-radius: $br;
      p{
        position: static;
        left: auto;
        margin: 0 1rem;
      }
      .clear-completed{
        cursor: pointer;
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
    font-family: 'Josefin Sans', sans-serif;
    .select-items{
      width: 100%;
      display: flex;
      flex-flow: row nowrap;
      justify-content: center;
      height: 2rem;
      padding: .5rem 0;
      align-items: center;
      cursor: pointer;
      border-radius: $br;
      color: $text-color1;
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
  .done{
    display: none !important;
  }
}

.checkbox-container{
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 20px;
  cursor: pointer;
  font-size: 22px;
  margin-left: 10px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  input{
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }
  .checkmark {
    position: absolute;
    top: -1px;
    left: 0;
    height: 16px;
    width: 16px;
    border-radius: 50%;
    border: 1px solid $bg-light;
    background-color: white ;
  }
}

.checkbox-container:hover input ~ .checkmark {
  background-color: #ccc;
}

.checkbox-container input:checked ~ .checkmark {
  //background-color: #2196F3;
  background-image: linear-gradient(-45deg,$gradient-color-2,$gradient-color-1);
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.checkbox-container input:checked ~ .checkmark:after {
  display: block;
}

.checkbox-container .checkmark:after {
  left: 6px;
  top: 3px;
  width: 3px;
  height: 6px;
  border: solid white;
  border-width: 0 2px 2px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

.active{
  color: blue !important;
}
.not-active{
  color: #9B9B9B;
}

</style>
