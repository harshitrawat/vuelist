<template>
  <div id="app" class="container">
    <h1 class="title">My To Do List</h1>
    <todo></todo>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Todo from './components/Todo.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    Todo,
  }
}
</script>

<style>

*{
  box-sizing: border-box;
}
body{
  background-color : white;
}
.container{
  margin: 0 auto;
  max-width: 600px;
}
.title{
  text-align: center;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 20px;
}
</style>
