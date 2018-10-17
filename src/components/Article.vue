<template>

  <div class="article">
      <!-- 懒加载 -->
          <div class="loader" v-if="isLoading">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
          </div>
      
    <div v-else class="article-inner">
        <div class="topic_header">
            
            <div class="topic_title"><span class="put_top">{{post | tabFormatter}}</span> 
            {{post.title}} </div>
            <ul>
                <li> • 发布于：{{post.create_at | formatDate}}</li>
                <li> • 作者：
                    {{post.author.loginname}}
                </li>
                <li> • {{post.visit_count}}次浏览 </li>
                <li> • 来自  {{post | tabFormatter}} </li>                
            </ul>
            <div class="topic_content" v-html="post.content" ></div>
        </div>
         <div id="reply" >
        <div >
            <div class="topbar">{{post.reply_count}} 回复</div>
            <div v-for="(reply,index) in post.replies" :key="index" class="reply">
                <div class="replyUp">
                <router-link :to="{
                    name:'user_info',
                    params:{
                        name:reply.author.loginname
                    }
                }">
                    <img :src="reply.author.avatar_url" alt="">
                </router-link>
                <router-link :to="{
                    name:'user_info',
                    params:{
                        name:reply.author.loginname
                    }
                }">
                    <span class="author">{{reply.author.loginname}}</span>
                </router-link>       
                <span class="floor">
                    {{index+1}}楼 • {{reply.create_at | formatDate}}
                </span>
                <span  v-if="reply.ups.length>0" class="thumbs">
                    <i class="fa fa-thumbs-o-up" title="点赞"></i>
                    {{reply.ups.length}}
                </span>
                
                <span v-else></span>
                </div>
            
                <p v-html="reply.content"></p>
            </div>
        </div>
        </div>
    </div>
 
  </div>
  
</template>

<script>
import loader from "./loader"
export default {
  name: "Article",
  data() {
    return {
      isLoading: false,
      post: {} //熟朋友内容属性
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then(res => {
          console.log(res);
          if (res.data.success == true) {
            this.isLoading = false;
            this.post = res.data.data;
          }
        })
        .catch(err => {
          console.error(err);
        });
    }
  },
  beforeMount: function() {
    this.isLoading = true; //加载成功之前显示加载动画
    this.getArticleData(); //页面加载前获取数据
  },
  watch: {
    $route(to, from) {
      this.getArticleData();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
@import url("../assets/markdown-github");


.article{
  margin: 0 13vw;
  margin-top: 15px;
  min-height: 400px;
  padding-bottom: 20px;
  padding-left: 30px
}
.article-inner {
  margin-right: 13vw;
}
.put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
}
.topic_header {
  padding: 10px;
  margin: 0 auto;
}

.topic_title {
  font-size: 22px;
  font-weight: 700;
  margin: 8px 0;
  display: inline-block;
  vertical-align: bottom;
  width: 75%;
  line-height: 130%;
}

.topic_header ul {
  padding: 0px 0px;
  margin: 6px 0px;
}

.topic_header li {
  display: inline-block;
  font-size: 12px;
  color: #838383;
}

.topic_content {
  border-top: 1px solid #e5e5e5;
  padding: 0 10px;
}

#reply {
  margin-top: 15px;
  padding-bottom: 8px;
}
#reply,
.topic_header {
  background-color: #fff;
}
#reply .reply {
  padding-left: 10px;
  background: #fff;
  border-top: 1px solid #f0f0f0;
}
#reply .floor {
  flex-grow: 1;
  color: #08c;
  text-decoration: none;
  margin: 0px 5px;
}
#reply .thumbs {
  margin-right: 20px;
  font-size: 15px;
}
#reply .replyUp {
  display: flex;
  align-items: center;
}
#reply .markdown-text{
    padding-left: 30px;
}
#reply .author{
    padding-left: 10px;
    color: #666;
    font-weight: 700;
}
#reply .topbar{
    padding: 10px;
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
}

#reply img {
  width: 30px;
  height: 30px;
  position: relative;
  bottom: -9px;
  border-radius: 10%;
}

#reply a,
#reply span {
  font-size: 13px;
  color: #666;
  text-decoration: none;
}
.replySec {
  border-bottom: 1px solid #e5e5e5;
  padding: 0 10px;
}

.loading {
  text-align: center;
  padding-top: 300px;
}

.replyUp a:nth-of-type(2) {
  margin-left: 0px;
  display: inline-block;
}

.markdown-text img {
  width: 92% !important;
  
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
