<template>
  <div class="UserInfo">
          <!-- 懒加载 -->
   <div class="loader" v-if="isLoading">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
          </div>
    <div class="userInfomation" v-else>
        <section>
            <img :src="userInfo.avatar_url" alt=""  :title="userInfo.loginname">
            <span class="loginname">{{userInfo.loginname}}</span>
            <p>
                积分：{{userInfo.score}}
            </p>
                <p>
                   GitHub： <a class="github" :href="href" target="_blank" :title="href">{{userInfo.githubUsername}}</a>
                </p>            
            <p class="creatTime">
                注册时间： {{userInfo.create_at | formatDate}}
            </p>
        </section>
        <div class="replies">
            <p>最近参与的话题</p>
            <ul>
                <li v-for="(item,index) in userInfo.recent_replies" :key="index">
                  <div class="item-inner">
                <img :src=item.author.avatar_url alt="" :title=item.author.loginname>
                <router-link :to="{
                    name:'post_content',
                    params:{                        
                        id:item.id,
                        name:item.author.loginname
                    }
                } ">
                    {{item.title}}
                </router-link>
                <span class="last_reply">{{item.last_reply_at | formatDate}} </span>
                </div>
                </li>
            </ul>
        </div>
        <div class="topics">
            <p>最近创建的话题</p>
            <ul>
                <li v-for="(item,index) in userInfo.recent_topics" :key="index">
                  <div class="item-inner">
                <img :src=item.author.avatar_url alt="" :title=item.author.loginname>
                <router-link :to="{
                    name:'post_content',
                    params:{                        
                        id:item.id,
                        name:item.author.loginname
                    }
                } ">
                    {{item.title}}
                </router-link>
                <span class="last_reply">{{item.last_reply_at | formatDate}} </span>                
                </div>                    
                </li>
            </ul>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "userInfo",
  data() {
    return {
      isLoading: false,
      userInfo: {},
      href: ""
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`, {
          params: {
            page: 1,
            limit: 42
          }
        })
        .then(res => {
          console.log(res);
          this.isLoading = false; //加载成功，去除动画
          this.userInfo = res.data.data;
          this.href = "https://github.com/" + this.userInfo.githubUsername;
        })
        .catch(function(err) {
          console.error(err);
        });
    }
  },
  beforeMount: function() {
    this.isLoading = true; //加载成功之前显示加载动画
    this.getData(); //页面加载前获取数据
  },
   watch: {
    $route(to, from) {
      this.getData();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.userInfomation {
  background: white;
  margin: 10px 20vw;
}
.userInfomation section {
  padding: 12px;
  position: relative;
}
.userInfomation > section > p{
  margin: 10px 0;
}
.userInfomation > section .creatTime{
    font-size: 14px;
  color: #778087;
}
.userInfomation img {
  width: 40px;
  height: 40px;
  border-radius: 3px;
}
.userInfomation li {
  list-style: none;
}
.userInfomation .replies,
.userInfomation .topics {
  font-size: 0.72rem;
  border-top: 10px #dddddd solid;
  padding-bottom: 20px;
}
.userInfomation > div > p {
  padding: 12px 0 12px 12px;
  background-color: rgba(212, 205, 205, 0.17);
  font-size: 0.75rem;
  margin: 0;
}

.userInfomation > div > ul > li {
  padding: 4px 0 4px 12px;
  white-space: nowrap;
  font-size: 12px;
  text-overflow: ellipsis;
  overflow: hidden;
  line-height: 30px;
  vertical-align: middle;
}
.userInfomation > div > ul > li  a {
  color: #094e99;
  text-decoration: none;
}
.github {
  color: cornflowerblue;
}
.loginname {
  font-size: 14px;
  line-height: 2em;
  color: #778087;
  padding-left: 10px;
  position: absolute;
  top: 10px;
}
.item-inner{
  display: flex;
  align-items: center;
  background: #fff;
  border-bottom: 1px solid #f0f0f0;
}
.item-inner a{
  flex-grow: 1;
  margin-left: 5px;
  padding:5px 0;
}
.item-inner .last_reply{
  font-size: 14px;
  margin-right: 15px;
  color: #777
}
.item-inner img{
  width: 30px;
  height:30px;
  margin-right: 10px;
}


.loader {
  position: fixed;
  top: 100px;
  left: 45%;
  transform: translate3d(-50%, -50%, 0);
}
.dot {
  width: 24px;
  height: 24px;
  background: #3ac;
  border-radius: 100%;
  display: inline-block;
  animation: slide 1s infinite;
}
.dot:nth-child(1) {
  animation-delay: 0.1s;
  background: #32aacc;
}
.dot:nth-child(2) {
  animation-delay: 0.2s;
  background: #64aacc;
}
.dot:nth-child(3) {
  animation-delay: 0.3s;
  background: #96aacc;
}
.dot:nth-child(4) {
  animation-delay: 0.4s;
  background: #c8aacc;
}
.dot:nth-child(5) {
  animation-delay: 0.5s;
  background: #faaacc;
}
@-moz-keyframes slide {
  0% {
    transform: scale(1);
  }
  50% {
    opacity: 0.3;
    transform: scale(2);
  }
  100% {
    transform: scale(1);
  }
}
@-webkit-keyframes slide {
  0% {
    transform: scale(1);
  }
  50% {
    opacity: 0.3;
    transform: scale(2);
  }
  100% {
    transform: scale(1);
  }
}
@-o-keyframes slide {
  0% {
    transform: scale(1);
  }
  50% {
    opacity: 0.3;
    transform: scale(2);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes slide {
  0% {
    transform: scale(1);
  }
  50% {
    opacity: 0.3;
    transform: scale(2);
  }
  100% {
    transform: scale(1);
  }
}
</style>
