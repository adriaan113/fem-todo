<template>
  <div class="list">
    <h1 class="test">{{test}}</h1>
    <ul>
      <li class="item" :class="[darkLight ? dark : light]">
        <!-- <div class="input-container"> -->
          <input type="checkbox" name="" id="">
          <span class="checkmark"></span>
          <p>{{thing}}</p>
          <span>x</span>
        <!-- </div> -->
      </li>
      <li class="item" :class="[darkLight ? dark : light]">
        <input type="checkbox" name="" id="">
        <p>niets</p>
        <span>x</span>
      </li>
      <li class="item" :class="[darkLight ? dark : light]">
        <input type="checkbox" name="" id="">
        <p>dit</p>
        <span>x</span>
      </li>
      <li class="item" :class="[darkLight ? dark : light]">
        <input type="checkbox" name="" id="">
        <p>dat</p>
        <span>x</span>
      </li>
      <li class="item" :class="[darkLight ? dark : light]">
        <input type="checkbox" name="" id="">
        <p>koeka</p>
        <span>x</span>
      </li>
      <li class="count-completed" :class="[darkLight ? dark : light]">
        <p> 5 items left</p>
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
    test: String,
    thing: String,
  },
  data: function(){
    return {
      night: true,
      dark: 'dark',
      light: 'light',
      darkLight: '',

      todo:'',
    }
  },
  methods:{
    okido(){
      if(this.test === 'dark'){
        this.darkLight = false;
      }else if(this.test === 'light'){
        this.darkLight = true;
      }
    }
  },
  updated(){
    this.okido();
  },
  mounted(){
    // this.test;
    this.okido();
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
      // .input-container{
      //   display: block;
      //   position: relative;
      //   padding-left: 35px;
      //   margin-bottom: 12px;
      //   cursor: pointer;
      //   font-size: 22px;
      //   -webkit-user-select: none;
      //   -moz-user-select: none;
      //   -ms-user-select: none;
      //   user-select: none;

      //   input[type="checkbox"]{
      //     position: absolute;
      //     opacity: 0;
      //     cursor: pointer;
      //     height: 0;
      //     width: 0;
      //     &:checked ~ .checkmark{
      //       background-color: blue;
      //       &:after{
      //         display: block;
      //       }
      //     }
      //   }
      //   .checkmark{
      //     position: absolute;
      //     top: 0;
      //     left: 0;
      //     height: 25px;
      //     width: 25px;
      //     background-color: red;
      //     // border-radius: 50%;
      //     &:after{
      //       content: '';
      //       position: absolute;
      //       display: none;
      //     }
      //     &:hover{
      //       background-color: green;
      //     }
      //   }
      //}

      
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

  .test{
    display: none;
  }
}

</style>
