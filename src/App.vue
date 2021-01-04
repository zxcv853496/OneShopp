<template>
  <div id="app">
    <div id="nav">
      <MainNav />
    </div>
    <router-view/>
  </div>
</template>



<script>
// @ is an alias to /src
import './assets/css/app.scss'
import MainNav from './components/MainNav/index'


export default {
  name: 'app',
  components: {
    MainNav,
  },
  data(){
    return{
    }
  }
}
</script>
