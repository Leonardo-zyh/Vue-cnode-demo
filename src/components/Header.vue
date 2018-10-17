<template>
<div class="header">
  <div class="header-inner"> 
    <router-link :to="{name:'root'}" >
      <img src="../assets/cnodejs_light.svg" class="brand">
    </router-link>          
    <ul>
        <router-link :to="{name:'root'}">
           <li><a href="#">首页</a></li>
        </router-link>              
        <li><a href="#">新手入门</a></li>
        <li><a href="#">API</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">注册</a></li>
        <li><a href="#">登录</a></li>

    </ul>
  </div>
</div>
</template>

<script>
export default {
  name: "Header"
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  background-color: #444;
  height: 50px;
  
}
.header-inner {
  height: 100%;
  width: 75%;
  margin: 0 auto;
  position: relative;
}
img {
  max-width: 120px;
  margin-left: 50px;
  margin-top: 10px;
}
ul {
  float: right;
  margin: 5px 5px;
  display: flex;
  align-items: center
  
}
li {
  display: inline-block;
}
a {
  color: #ccc;
  font-size: 14px;
  padding: 10px 15px;
  text-shadow: none;
}

.brand{
  position: absolute;
  left:-1vw;
}
</style>
