<template>

  <div id="app">
    <Header></Header>
    <div class="main">
       <router-view name="main"></router-view>       
    </div>
    <div class="sildeBar">
      <router-view name="sildeBar"></router-view>
    </div>
   
    <!-- <router-view/> -->
  </div>
</template>

<script>

import Header from '@/components/Header'
import PostList from '@/components/PostList'
export default {
  name: 'App',
  components:{
    Header,
  }
}
</script>

<style>

@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css");
*{margin:0;padding:0;font-family: Helvetica, sans-serif;}
*{box-sizing: border-box;}
*::after{box-sizing: border-box;}
*::before{box-sizing: border-box;}
a{color: inherit; text-decoration: none;}
ul{list-style: none;}
#app{
  background-color: #e1e1e1;
}

.main{
  position: relative;
}
.sildeBar{
  position: absolute;
  top:65px;
  /* right:13vw; */
  right: 5vw
}

</style>
