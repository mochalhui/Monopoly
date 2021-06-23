<template>
  <div id="container">
    <Layout></Layout>
  </div>
</template>

<script>
import Layout from './components/Layout.vue'

export default {
  name: 'App',
  components: {
    Layout
  },
  methods:{
    hello(){
     
    }
  }
}
</script>

<style>
#container{
  width:1200px;
  height: 900px;
  position: relative;
  top: 80px;
  margin: 0 auto;
  display: grid;
  grid-template-rows: repeat(9,100px);
  grid-template-columns: repeat(12,100px);
  background: lightblue;
  text-align: center;
  line-height: 100px;
}

</style>
