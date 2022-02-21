<template>
  <!-- <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>|
    <router-link to="/test-chart">Test Chart</router-link>
  </div> -->

  <Header/>
  <div class="container">
    <router-view/>
  </div>
</template>

<script>
import Header from '@/components/Header'

export default {
  components: {
    Header,
  },
}
</script>


<style>

</style>
