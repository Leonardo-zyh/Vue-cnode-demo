<template>
  <div class="postList"> 
      <div class="loading" v-if="isLoading">
          <img  src="../assets/loading.gif" alt="">
      </div>
      <!-- 主题帖子列表 -->
      <div v-else>
          <ul>
              <li>
              <div class="topbar">
              <span v-for="(item,index) in topbar" :key="index"
               @click="topbarClick(item)" 
               :class="[{click:item == click},] "
               >{{item}}</span>
              </div>
              </li>
              <li v-for="(post,index) in posts" :key="index">
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
                <span class="title">
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
      posts: [], //页面列表数组
      postpage: 1,
      topbar:['全部','精华','分享','问答','招聘'],
      tab:'',
      click:'',
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
            limit: 20
          }
        })
        .then(res => {
          console.log(res);
          this.isLoading = false; //加载成功，去除动画
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
    topbarClick(e){
       this.click = e
       this.tab = e
       
       if(e='全部'){
         this.tab = 'all'
       }else if(e= '精华'){
         this.tab = 'ture'
       }else if(e='分享'){
         this.tab = 'share'
       }else if(e='问答'){
         this.tab = 'ask'
       }else if(e='招聘'){
         this.tab = 'recruit'
       }
       console.log(this.tab);                     
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

.postList {
  margin: 8px 50px;
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

ul {
  list-style: none;
  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
  box-shadow: 0 0 1px 0 #999;
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

.topbar>span {  
  color: #80bd01;
  padding: 3px 4px;
  border-radius: 3px;
  margin: 0 10px;
  cursor: pointer;
}
.topbar>span.click{
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

.loading {
  text-align: center;
  padding-top: 300px;
  height: 100vw;
}
.loading img{
  width: 10vw;
  height: 10vw;
  border-radius: 50%
}
</style>
