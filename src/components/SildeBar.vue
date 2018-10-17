<template>
  <div class="loader" v-if="isLoading">
      <!-- <div class="loader__bar"></div>
      <div class="loader__bar"></div>
      <div class="loader__bar"></div>
      <div class="loader__bar"></div>
      <div class="loader__bar"></div>
      <div class="loader__ball"></div> -->
  </div>
  <div class="autherinfo"  v-else>
      <div class="authersummay">
          <div class="topbar">作者</div>
            <div class="topbar-inner">
                <router-link :to="{
                    name:'user_info',
                    params:{
                        name:userinfo.loginname
                    }
                }" class="img-wrapper">
                    <img :src="userinfo.avatar_url" alt="" :title=userinfo.loginname>                
                <div class="loginname">{{userinfo.loginname}}</div>
                </router-link>
                <div class="score">积分： {{userinfo.score}}</div>
                <div class="githubUsername">
                   GitHub： <a class="github" :href="href" target="_blank" :title="href">{{userinfo.githubUsername}}</a>
                </div>
              </div>
               
      </div>
      <div class="recent_topics">
          <div class="topbar">作者其他话题</div>
          <ul>
              <li v-for="(list,index) in topclimitby5" :key="index" :title="list.title">
                <router-link :to="{
                    name:'post_content',
                    params:{                        
                        id:list.id,
                        name:list.author.loginname
                    }
                } ">
                    {{list.title}}
                </router-link>
              </li>
          </ul>
      </div>
      <div class="recent_replies">
          <div class="topbar">作者最近回复话题</div>
                    <ul>
              <li v-for="(list,index) in replylimitby5" :key="index" :title="list.title">
                <router-link :to="{
                    name:'post_content',
                    params:{                        
                        id:list.id,
                        name:list.author.loginname
                    }
                } ">
                    {{list.title}}
                </router-link>
              </li>
          </ul>
      </div>
  </div>
</template>

<script>
export default {
  name: "SildeBar",
  data() {
    return {
      isLoading: false,
      userinfo: {},
      href: ""
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          console.log(res);
          this.isLoading = false;
          this.userinfo = res.data.data;
          this.href = "https://github.com/" + this.userinfo.githubUsername;
        })
        .catch(function(err) {
          console.error(err);
        });
    }
  },
  computed: {
    topclimitby5() {
      if (this.userinfo.recent_topics) {
        //length还未产生，使用报错
        return this.userinfo.recent_topics.slice(0, 5);
      } else {
        return this.userinfo.recent_topics;
      }
    },
    replylimitby5() {
      if (this.userinfo.recent_replies) {
        return this.userinfo.recent_replies.slice(0, 5);
      } else {
        return this.userinfo.recent_replies;
      }
    }
  },
  beforeMount: function() {
    this.isLoading = true;
    this.getData(); //页面加载前获取数据
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.recent_replies,
.recent_topics {
  background-color: #fff;
  border-radius: 5px;
  margin-top: 10px;
}
.authersummay {
  background-color: #fff;
  border-radius: 5px;
}
.autherinfo {
  width: 20vw;
  float: right;
  margin-top: 0;
}
li {
  padding: 4px 0;
}
.recent_replies ul,
.recent_topics ul {
  margin-top: 0px;
  margin-bottom: 0px;
  list-style: none;
  padding: 10px;
}

ul a {
  max-width: 270px;
  font-size: 14px;
  text-decoration: none;
  color: #778087;
}

.topbar {
  padding: 10px;
  background-color: #f6f6f6;
  height: 40px;
  font-size: 14px;
  border-radius: 5px;
  margin-bottom: 10px;
}
.topbar-inner {
  padding: 5px 10px 10px 10px;
}
.topbar-inner .score,
.githubUsername {
  padding: 5px 0;
}

.authersummay img {
  height: 48px;
  width: 48px;
  border-radius: 3px;
}
.authersummay .img-wrapper {
  display: flex;
  align-items: center;
}
.authersummay .loginname {
  display: inline-block;
  color: #778087;
  font-size: 16px;
  padding-left: 10px;
}

.loginname a {
  text-decoration: none;
  color: #778087;
}

.authersummay .topbar {
  margin-top: 0px;
}
.github {
  color: cornflowerblue;
}



.loader {
  position: relative;
  margin-right: 20vw;
  width: 75px;
  height: 100px;
}
.loader__bar {
  position: absolute;
  bottom: 0;
  width: 10px;
  height: 50%;
  background: #EA4961;
  -webkit-transform-origin: center bottom;
  transform-origin: center bottom;
  box-shadow: 1px 1px 0 rgba(0, 0, 0, 0.2);
}
.loader__bar:nth-child(1) {
  left: 0px;
  -webkit-transform: scale(1, 0.2);
  transform: scale(1, 0.2);
  -webkit-animation: barUp1 1s infinite;
  animation: barUp1 1s infinite;
}
.loader__bar:nth-child(2) {
  left: 15px;
  -webkit-transform: scale(1, 0.4);
  transform: scale(1, 0.4);
  -webkit-animation: barUp2 1s infinite;
  animation: barUp2 1s infinite;
}
.loader__bar:nth-child(3) {
  left: 30px;
  -webkit-transform: scale(1, 0.6);
  transform: scale(1, 0.6);
  -webkit-animation: barUp3 1s infinite;
  animation: barUp3 1s infinite;
}
.loader__bar:nth-child(4) {
  left: 45px;
  -webkit-transform: scale(1, 0.8);
  transform: scale(1, 0.8);
  -webkit-animation: barUp4 1s infinite;
  animation: barUp4 1s infinite;
}
.loader__bar:nth-child(5) {
  left: 60px;
  -webkit-transform: scale(1, 1);
  transform: scale(1, 1);
  -webkit-animation: barUp5 1s infinite;
  animation: barUp5 1s infinite;
}
.loader__ball {
  position: absolute;
  bottom: 10px;
  left: 0;
  width: 10px;
  height: 10px;
  background: #EA4961;
  border-radius: 50%;
  -webkit-animation: ball 1s infinite;
  animation: ball 1s infinite;
}

@-webkit-keyframes ball {
  0% {
    -webkit-transform: translate(0, 0);
    transform: translate(0, 0);
  }
  5% {
    -webkit-transform: translate(8px, -14px);
    transform: translate(8px, -14px);
  }
  10% {
    -webkit-transform: translate(15px, -10px);
    transform: translate(15px, -10px);
  }
  17% {
    -webkit-transform: translate(23px, -24px);
    transform: translate(23px, -24px);
  }
  20% {
    -webkit-transform: translate(30px, -20px);
    transform: translate(30px, -20px);
  }
  27% {
    -webkit-transform: translate(38px, -34px);
    transform: translate(38px, -34px);
  }
  30% {
    -webkit-transform: translate(45px, -30px);
    transform: translate(45px, -30px);
  }
  37% {
    -webkit-transform: translate(53px, -44px);
    transform: translate(53px, -44px);
  }
  40% {
    -webkit-transform: translate(60px, -40px);
    transform: translate(60px, -40px);
  }
  50% {
    -webkit-transform: translate(60px, 0);
    transform: translate(60px, 0);
  }
  57% {
    -webkit-transform: translate(53px, -14px);
    transform: translate(53px, -14px);
  }
  60% {
    -webkit-transform: translate(45px, -10px);
    transform: translate(45px, -10px);
  }
  67% {
    -webkit-transform: translate(37px, -24px);
    transform: translate(37px, -24px);
  }
  70% {
    -webkit-transform: translate(30px, -20px);
    transform: translate(30px, -20px);
  }
  77% {
    -webkit-transform: translate(22px, -34px);
    transform: translate(22px, -34px);
  }
  80% {
    -webkit-transform: translate(15px, -30px);
    transform: translate(15px, -30px);
  }
  87% {
    -webkit-transform: translate(7px, -44px);
    transform: translate(7px, -44px);
  }
  90% {
    -webkit-transform: translate(0, -40px);
    transform: translate(0, -40px);
  }
  100% {
    -webkit-transform: translate(0, 0);
    transform: translate(0, 0);
  }
}

@keyframes ball {
  0% {
    -webkit-transform: translate(0, 0);
    transform: translate(0, 0);
  }
  5% {
    -webkit-transform: translate(8px, -14px);
    transform: translate(8px, -14px);
  }
  10% {
    -webkit-transform: translate(15px, -10px);
    transform: translate(15px, -10px);
  }
  17% {
    -webkit-transform: translate(23px, -24px);
    transform: translate(23px, -24px);
  }
  20% {
    -webkit-transform: translate(30px, -20px);
    transform: translate(30px, -20px);
  }
  27% {
    -webkit-transform: translate(38px, -34px);
    transform: translate(38px, -34px);
  }
  30% {
    -webkit-transform: translate(45px, -30px);
    transform: translate(45px, -30px);
  }
  37% {
    -webkit-transform: translate(53px, -44px);
    transform: translate(53px, -44px);
  }
  40% {
    -webkit-transform: translate(60px, -40px);
    transform: translate(60px, -40px);
  }
  50% {
    -webkit-transform: translate(60px, 0);
    transform: translate(60px, 0);
  }
  57% {
    -webkit-transform: translate(53px, -14px);
    transform: translate(53px, -14px);
  }
  60% {
    -webkit-transform: translate(45px, -10px);
    transform: translate(45px, -10px);
  }
  67% {
    -webkit-transform: translate(37px, -24px);
    transform: translate(37px, -24px);
  }
  70% {
    -webkit-transform: translate(30px, -20px);
    transform: translate(30px, -20px);
  }
  77% {
    -webkit-transform: translate(22px, -34px);
    transform: translate(22px, -34px);
  }
  80% {
    -webkit-transform: translate(15px, -30px);
    transform: translate(15px, -30px);
  }
  87% {
    -webkit-transform: translate(7px, -44px);
    transform: translate(7px, -44px);
  }
  90% {
    -webkit-transform: translate(0, -40px);
    transform: translate(0, -40px);
  }
  100% {
    -webkit-transform: translate(0, 0);
    transform: translate(0, 0);
  }
}
@-webkit-keyframes barUp1 {
  0% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  40% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  50% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  90% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  100% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
}
@keyframes barUp1 {
  0% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  40% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  50% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  90% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  100% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
}
@-webkit-keyframes barUp2 {
  0% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  40% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  50% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  90% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  100% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
}
@keyframes barUp2 {
  0% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  40% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  50% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  90% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  100% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
}
@-webkit-keyframes barUp3 {
  0% {
    -webkit-transform: scale(1, 0.6);
    transform: scale(1, 0.6);
  }
  100% {
    -webkit-transform: scale(1, 0.6);
    transform: scale(1, 0.6);
  }
}
@keyframes barUp3 {
  0% {
    -webkit-transform: scale(1, 0.6);
    transform: scale(1, 0.6);
  }
  100% {
    -webkit-transform: scale(1, 0.6);
    transform: scale(1, 0.6);
  }
}
@-webkit-keyframes barUp4 {
  0% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  40% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  50% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  90% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  100% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
}
@keyframes barUp4 {
  0% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  40% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
  50% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  90% {
    -webkit-transform: scale(1, 0.4);
    transform: scale(1, 0.4);
  }
  100% {
    -webkit-transform: scale(1, 0.8);
    transform: scale(1, 0.8);
  }
}
@-webkit-keyframes barUp5 {
  0% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  40% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  50% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  90% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  100% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
}
@keyframes barUp5 {
  0% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  40% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
  50% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  90% {
    -webkit-transform: scale(1, 0.2);
    transform: scale(1, 0.2);
  }
  100% {
    -webkit-transform: scale(1, 1);
    transform: scale(1, 1);
  }
}
</style>
