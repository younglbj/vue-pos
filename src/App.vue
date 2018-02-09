<template>
  <div id="app">
    <leftNav></leftNav>
    <div class="main">
      <router-view/>
    </div>
  </div>
</template>

<script>
  import leftNav from '@/components/common/leftNav'
  export default {
    name: 'App',
    components:{
      leftNav
    }
  }
</script>

<style>
  html,body,ul,#app{height:100%;padding: 0;margin:0;}
  ul li{list-style: none;}
   #app {
     font-family: 'Microsoft Yahei', Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     text-align: center;
     color: #2c3e50;
   }
  .main{
    height:100%;
    width:calc(100% - 68px);
    /*min-width:950px;*/
    background: #EFF2F7;
    float:left;
    overflow: auto;

  }
</style>
