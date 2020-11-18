<template>
  <div class="home">
<!--    <img alt="Vue logo" src="../assets/logo.png">-->
<!--    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <Barrage/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Barrage from '@/components/Barrage.vue'

export default {
  name: 'Home',
  components: {
    // HelloWorld,
    Barrage
  }
}
</script>
