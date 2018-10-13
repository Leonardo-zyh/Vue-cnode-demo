<template>
  <div class="autherinfo">
      <div class="authersummay">
          <div class="topbar">作者</div>
                <router-link :to="{
                    name:'user_info',
                    params:{
                        name:userinfo.loginname
                    }
                }">
                    <img :src="userinfo.avatar_url" alt="">                
                <div class="loginname">{{userinfo.loginname}}</div>
                </router-link>
                <div>积分: {{userinfo.score}}</div>
                <div>
                   GitHub： <a class="github" href="https://github.com/alsotang" target="_blank">{{userinfo.githubUsername}}</a>
                </div>
               
      </div>
      <div class="recent_topics">
          <div class="topbar">作者其他话题</div>
          <ul>
              <li v-for="(list,index) in topclimitby5" :key="index">
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
              <li v-for="(list,index) in replylimitby5" :key="index">
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
      userinfo: {}
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          console.log(res);
          this.userinfo = res.data.data;
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
  padding: 10px;
}
.autherinfo {
  width: 328px;
  float: right;
  margin-top: 0;
}
li {
  padding: 3px 0;
}
.recent_replies ul,
.recent_topics ul {
  margin-top: 0px;
  margin-bottom: 0px;
  list-style: none;
  padding-left: 14px;
}

ul a {
  font-size: 12px;
  text-decoration: none;
  color: #778087;
}

.topbar {
  padding: 10px;
  background-color: #f6f6f6;
  height: 40px;
  font-size: 12px;
  border-radius: 5px;
  margin-bottom: 10px;
}

.authersummay img {
  height: 48px;
  width: 48px;
  border-radius: 3px;
}

.authersummay .loginname {

  width: 100px;
  float: right;
  margin-top: 22px;
  margin-right: 159px;
  font-size: 14px;
  color: #778087;
  font-size: 16px;
  padding-left: 10px
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
</style>
