<template>
 <div id="nav">
   <router-link to="/adddata"><button type="button" class="btn btn-secondary">Adddata</button></router-link>&nbsp;&nbsp;
   <router-link to="/getdata"><button type="button" class="btn btn-secondary">GetData</button></router-link><br><br>

   <router-view/>
  <HelloWorld/>
 

   </div>

</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
