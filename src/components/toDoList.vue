<template>
  <div class="list">
    <ul>
      <li>
       <input type="radio" name="" id="">
       <p>iets</p>
       <span>x</span>
      </li>
      <li>
        <input type="radio" name="" id="">
        <p>niets</p>
        <span>x</span>
      </li>
      <li>
        <input type="radio" name="" id="">
        <p>dit</p>
        <span>x</span>
      </li>
      <li>
        <input type="radio" name="" id="">
        <p>dat</p>
        <span>x</span>
      </li>
      <li>
        <input type="radio" name="" id="">
        <p>koeka</p>
        <span>x</span>
      </li>
    </ul>
    <div>
      <p>5 items left</p>
      <p>all</p>
      <p>active</p> 
      <p>completed</p>
      <p>clear completed</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'toDoList',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss"  scoped>


</style>
