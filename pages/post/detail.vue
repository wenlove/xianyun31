<template>
  <!-- 攻略详情页 -->
  <section class="container">
    <el-row type="flex" justify="space-between">
      <div class="main">
        <!-- 面包屑导航 -->
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item :to="{ path: '/post' }">游戏攻略</el-breadcrumb-item>
          <el-breadcrumb-item>攻略详情</el-breadcrumb-item>
        </el-breadcrumb>

        <!-- 标题 , 信息-->
        <h1>{{article.title}}</h1>
        <div class="post-info">
          <span>攻略：{{city.created_at}}</span>
          <span>阅读：{{article.watch}}</span>
        </div>

        <!-- 详细内容 -->
        <div class="post-content" v-html="article.content">{{article.content}}</div>

        <div>
          <Comments />
        </div>

        <!-- 提交评论 -->
        <div>
          <PostComments />
        </div>

        <!-- 获取评论 -->
        <GetComment />
      </div>

      <div class="aside">
        <h4>相关攻略</h4>
      </div>
    </el-row>
  </section>
</template>

<script>
import Comments from "@/components/post/comment.vue";
import PostComments from "@/components/post/PostComments.vue";
import GetComment from "@/components/post/GetComment.vue";
export default {
  data() {
    return {
      article: {
        title: "",
        content: "",
        watch: ""
      }, //文章详情信息总数据

      account: {}, //z账户信息
      city: {
        created_at: ""
      },
      Comments: {} //评论信息
    };
  },
  components: {
    Comments,
    PostComments,
    GetComment
  },
  mounted() {
    const id = this.$route.query.id;
    this.$axios({
      url: "/posts?id=" + id
    }).then(res => {
      this.article = res.data.data[0];
      const { account, city, Comments } = res.data.data[0];
      this.account = account;
      this.city = city;
      this.Comments = Comments;

      // console.log(this.article);
    });
  }
};
</script>

<style lang="less" scoped>
.container {
  width: 1000px;
  margin: 0 auto;
  padding: 20px 0;
  .main {
    width: 700px;

    h1 {
      padding: 20px 0;
      border-bottom: 1px solid #ddd;
      font-size: 32px;
    }
    .post-info {
      padding: 20px 0;
      color: #999;
      text-align: right;
    }
  }

  .aside {
    width: 280px;
    height: 300px;
    h4 {
      border-bottom: 1px solid #ddd;
      color: #999;
    }
  }
}
</style>
