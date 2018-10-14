<template>
  <div class="UserInfo">
          <!-- 懒加载 -->
    <div class="loading" v-if="isLoading">
          <img src="../assets/loading.gif" alt="">
      </div>
    <div class="userInfomation" v-else>
        <section>
            <img :src="userInfo.avatar_url" alt="">
            <span class="loginname">{{userInfo.loginname}}</span>
            <p>
                积分：{{userInfo.score}}
            </p>
                <p>
                   GitHub： <a class="github" :href="href" target="_blank">{{userInfo.githubUsername}}</a>
                </p>            
            <p class="creatTime">
                注册时间： {{userInfo.create_at | formatDate}}
            </p>
        </section>
        <div class="replies">
            <p>最近参与的话题</p>
            <ul>
                <li v-for="(item,index) in userInfo.recent_replies" :key="index">
                <router-link :to="{
                    name:'post_content',
                    params:{                        
                        id:item.id
                    }
                } ">
                    {{item.title}}
                </router-link>
                </li>
            </ul>
        </div>
        <div class="topics">
            <p>最近创建的话题</p>
            <ul>
                <li v-for="(item,index) in userInfo.recent_topics" :key="index">
                <router-link :to="{
                    name:'post_content',
                    params:{                        
                        id:item.id
                    }
                } ">
                    {{item.title}}
                </router-link>                    
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
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.userInfomation {
  background: white;
  width: 75%;
  margin: 10px auto;
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
  font-size: 0.72rem;
  text-overflow: ellipsis;
  overflow: hidden;
  line-height: 30px;
  vertical-align: middle;
}
.userInfomation > div > ul > li > a {
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
</style>
