<template>
  <div class="list">
    <h1 class="color-status">{{colorStatus}}{{thing}}{{itemsLeft}}</h1>
    <ul class="list-cotainer" >
      <li class="item" 
          :class="[darkLight ? dark : light]" 
          v-for="(item,index) in alfie" 
          :key="item.id" 
          :counter="itemsLeft"
          draggable
          @dragover="(e) => onDragOver(item, index, e)" 
          @dragend="(e) => finishDrag(item, index, e)"
          @dragstart="(e) => startDrag(item, index, e)"
          >
          <input type="checkbox" name="" id="checkbox"  v-model="item.completed">
          <label for="checkbox" :class="{completed: item.completed}">{{item.msg}}</label>
          <span @click="deleteTodo(item,index)">x</span>
      </li>
      <li class="count-completed" :class="[darkLight ? dark : light]">
        <p v-if="counter != 0"> {{counter}} items left</p>
        <p v-else>All done!</p>
        <p class="clear-completed" @click="clearCompleted"> clear completed</p>
      </li>
    </ul>
    <!-- <ul class="control">
      <li class="select-items" :class="[darkLight ? dark : light]">all</li>
      <li class="select-items" :class="[darkLight ? dark : light]">active</li>
      <li class="select-items" :class="[darkLight ? dark : light]">completed</li>
    </ul> -->
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
      //alfie: this.thing, 
      result: [],
      //alfie:'',
      // alfie: this.thing.map(x => ({...x})),
  

      isChecked: false,

      over: {},
      startLoc: 0,
      dragging: false,
      dragFrom: {}
    }
  },  
  computed:{
    counter(){
      return this.alfie.length;
    },
    alfie(){
      return this.thing.map(x => ({...x}));
       
      //return this.thing;
    }
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
      if ( this.alfie.indexOf(name) === idx) { 
        this.alfie.splice(idx, 1); 
        this.$forceUpdate();//niet heel mooi toch dit? maar ik kon even niets anders verzinnen. Reactivity confusion
      }
      
    },
    checkThis(name,idx){
      if ( this.alfie.indexOf(name) === idx) { 
        return this.alfie.splice(idx, 1); 
      }
    },
    clearCompleted(){
      for(let i = this.alfie.length -1; i>=0;--i){
        if(this.alfie[i].completed){
          this.alfie.splice(i,1);
        }
      }
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
      cursor: grab;
      &:last-child{
        border-bottom: none;
      }
      input,label,span{
        margin: 0 1rem;
      }
      span{
        cursor: pointer;
      }
      label{
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
