<template>
  <div id="container">
    <h4>评论</h4>
    <div>
      <!-- 输入框 -->
      <el-input type="textarea" :rows="2" placeholder="说点什么吧..." v-model="content" resize="none"></el-input>
    </div>

    <div class="box">
      <div>
        <!-- 提交按钮 -->
        <el-tag
          v-for="item in items"
          :key="item.label"
          :type="item.type"
          effect="dark"
          @click="handleClickSubmit"
          class="inputSub"
        >{{ item.label }}</el-tag>
      </div>
      <!-- 照片墙 -->
      <div>
        <el-upload
          :action="`${$axios.defaults.baseURL}/upload`"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
          name="files"
          :on-success="handleSuccess"
          :headers="{Authorization:[$store.state.user.userInfo.token.password]}"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt />
        </el-dialog>
        <div></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogImageUrl: "",
      dialogVisible: false,

      imgURL: [],
      pic: [],
      content: "",
      post: 0, //评论者ID
      follow: 1, //回复ID

      // 提交按钮
      items: [{ type: "", label: "提交" }]
    };
  },
  methods: {
    //点击删除图片时触发
    handleRemove(file, fileList) {
      // console.log(123);
    },
    //点击预览图片时触发
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },

    //上传成功时触发
    handleSuccess(response, file, fileList) {
      this.imgURL = fileList.map(v => {
        return v.response[0];
      });
      console.log(this.imgURL);
    },

    handleClickSubmit() {
      console.log(123)
      const { token } = this.$store.state.user.userInfo;
      this.$axios({
        url: "/comments",
        method: "POST",
        data: {
          pics: this.imgURL,
          content: this.content,
          post:4, //评论者ID
          follow: 39 //回复ID
        },
        headers: {
          Authorization: `bearer ${token}`,
          "Content-Type": "application/json"
        }
      }).then(res => {
        const { status, message } = res.data;
        if (status == 0) {
          this.$message.success(message);
        }
        console.log(res, 123);
        const id = this.$route.query.id;
        // this.$axios({
        //   url: "/posts/comments",
        //   params: {
        //     post: id,
        //     _limit: 20,
        //     _start: 0
        //   }
        // }).then(res => {
        //   console.log(res, "这里");
        // });
      });
    }
  }
};
</script>

<style>
#container {
  width: 800px;
}
h4 {
  font-size: 18px;
  margin-bottom: 20px;
  font-weight: 400;
}
.box {
  padding: 20px 0;
}
.inputSub {
  float: right;
}
</style>
