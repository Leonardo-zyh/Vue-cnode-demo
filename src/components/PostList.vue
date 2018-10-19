<template>
  <div class="postList"> 
     
          <div class="loader" v-if="isLoading">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
          </div>
    
      <!-- 主题帖子列表 -->
      <div v-else class="postList-inner">
          <ul>
              <li>
              <div class="topbar"  v-if="isTopbarLoading">
                <span @click="topbarClick('',1)" :class="{click:isShow === 1}">全部</span>
                <span @click="topbarClick('good',2)" :class="{click:isShow === 2}">精华</span>
                <span @click="topbarClick('share',3)" :class="{click:isShow === 3}">分享</span>
                <span @click="topbarClick('ask',4)" :class="{click:isShow === 4}">问答</span>
                <span @click="topbarClick('job',5)" :class="{click:isShow === 5}">招聘</span>
                <span @click="topbarClick('dev',6)" :class="{click:isShow === 6}">测试</span>
              </div>
              </li>
              <li v-for="(post,index) in posts" :key="index" >
                <!-- 头像 -->
                <router-link :to="{
                    name:'user_info',
                    params:{
                        name:post.author.loginname
                    }
                }">
                    <img :src="post.author.avatar_url" alt="">
                </router-link>
                <!-- 浏览量 -->
                <span class="count">
                  <span class="reply_count">{{post.reply_count}}</span>/<span class="reply_count">{{post.visit_count}}</span>                  
                </span>
                <!-- 帖子分类 -->
                <span :class="[{'put_good':(post.good == true),'put_top':(post.top==true),'topiclist-tab':(post.good != true && post.top != true)}]" >
                {{post | tabFormatter}}
                </span>

                <!-- 标题 -->
                <router-link :to="{
                  name:'post_content',
                  params:{
                    id:post.id,
                    name:post.author.loginname
                  }}">
                <span class="title" :title=post.title>
                  {{post.title}}
                </span>
                
                <span class="timeLastReply">
                  {{post.last_reply_at | formatDate}}
                </span>
                </router-link>
              <li>
              <pagination @handleList="renderList"></pagination>
              </li>
          </ul>
      </div>
  </div>
</template>

<script>
import pagination from "./Pagination";

export default {
  name: "PostList",
  data() {
    return {
      isLoading: false,
      isTopbarLoading: false,
      posts: [], //页面列表数组
      postpage: 1,
      tab: "",
      pageNumber: 1,
      isShow: 1
    };
  },
  components: {
    pagination
  },
  methods: {
    getData() {
      this.$http
        .get("https://cnodejs.org/api/v1/topics", {
          params: {
            //get请求需要些params参数
            page: this.postpage,
            limit: 20,
            tab: this.tab
          }
        })
        .then(res => {
          console.log(res);
          this.isLoading = false; //加载成功，去除动画
          this.isTopbarLoading = true;
          this.posts = res.data.data;
        })
        .catch(function(err) {
          console.error(err);
        });
    },
    renderList(value) {
      this.postpage = value;
      this.getData();
    },
    topbarClick(value, num) {
      this.tab = value;
      this.isShow = num;
      this.getData();
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
<style lang='scss' scoped>
.postList {
  margin: 8px auto;
  font-size: 16px;
}
.posts {
  margin-top: 10px;
}

.postList img {
  height: 30px;
  width: 30px;
  border-radius: 3px;
  vertical-align: middle;
}
.postList .postList-inner {
  margin: 0 auto;
}
ul {
  list-style: none;
  box-shadow: 0 0 1px 0 #999; 
  margin: 0 15vw;
}

ul li:not(:first-child) {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma,
    "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
}

li:not(:first-child):hover {
  background: #f5f5f5;
}

li:last-child:hover {
  background: white;
}

li span {
  line-height: 30px;
}
.title {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  display: inline-block;
  max-width: 75%;
  vertical-align: middle;
  font-size: 16px;
}

.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
}
.count {
  width: 70px;
  display: inline-block;
  text-align: center;
}
.reply_count {
  color: #9e78c0;
  font-size: 14px;
}

.put_good,
.put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}

.topiclist-tab {
  background-color: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}

.last_reply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}
.timeLastReply {
  float: right;
  min-width: 50px;
  display: inline-block;
  color: #778087;
  white-space: nowrap;
}

.topbar {
  height: 100%;
  padding: 10px 5px;
  background-color: #f6f6f6;
  border-radius: 3px 3px 0 0;
  margin: 0;
}

.topbar > span {
  color: #80bd01;
  padding: 3px 4px;
  border-radius: 3px;
  margin: 0 10px;
  cursor: pointer;
}
.topbar > span.click {
  background-color: #80bd01;
  color: #fff;
}

.topbar span:hover {
  color: #9e78c0;
}

a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
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
