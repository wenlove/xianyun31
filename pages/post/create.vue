<template>
    <div class="container">
        <el-row type="flex" justify="space-between">

            <!-- 左边栏开始 -->
            <div class="box-left">
                <h2>发表新攻略</h2>
                <p>分享你的个人游记，让更多人看到哦！</p>

                <!-- 标题 -->
                <el-input 
                    v-model="form.title"
                    placeholder="请输入内容"
                    class="input-title">
                </el-input>
                <!-- 标题结束 -->

                <!-- 富文本框 -->
                <div class="textbox">
                    <quill-editor  v-model="form.content"></quill-editor>
                </div>
                <!-- 富文本框结束 -->

                <!-- 选择城市 -->
                <div>
                    选择城市
                    <el-autocomplete
                        class="input-city"
                        v-model="form.city"
                        :fetch-suggestions="queryDepartSearch"
                        placeholder="请搜索出发城市"
                        @select="handleDepartSelect">
                    </el-autocomplete>
                </div>
                <!-- 选择城市结束 -->


                <!-- '发布'或者'保存到草稿' -->
                <el-button type="primary" @click="handleSubmit">发布</el-button>
                     　and 
                <el-button type="warning" @click="handleAddDraft">保存到草稿</el-button>
                <!-- 按钮结束 -->
            </div>
            <!-- 左边栏结束 -->


            <!-- 右边栏（草稿箱） -->
            <div class="box-right">
                <div class="draft">
                    <h4>草稿箱({{draft.length}})</h4>
                    <div class="draft-1" v-for="(item,index) in draft" :key="index">
                        <div>
                            <div class="draft-2" >
                                <a @click="handleDel">删除</a>
                                <div @click="handleEdit(index)">
                                    {{item.title}}  
                                    <i class="el-icon-edit"></i>
                                </div>
                            </div>
                            <p>{{systemDate}}</p>
                        </div>
                    </div>
                </div>
            </div>
            <!-- 右边栏结束 -->
        </el-row>
    </div>
</template>

<script>
  export default {
    data () {
      return {
        editorOption: {
          // some quill options
          modules: {
            toolbar: [
              ['bold', 'italic', 'underline', 'strike'],
              ['blockquote', 'code-block']
            ]
          }
        },
        form:{
            title:'德玛西亚万岁',
            content: '',
            city:"广州市",
        },
        systemDate:'',
        draft:[
            {title:'boom',content: '原地爆炸',city:"广州市"}
        ],
      }
    },
    mounted() {
      console.log('123',this.$store.state.user.userInfo.token);
      this.draft = JSON.parse(localStorage.getItem("airs") || `[]`);

      
        let nowDate = new Date();
        let date = {
            year: nowDate.getFullYear(),
            month: nowDate.getMonth() + 1,
            date: nowDate.getDate(),
        }
        this.systemDate = date.year + '-' + 0 + date.month + '-' + 0 + date.date;
    },
    methods: {
      handleAddDraft(){
            if(!this.form.title||!this.form.content||!this.form.city){
               this.$message.warning('请输入完整的信息')
               return
            }

            const obj = {...this.form};

            
            this.draft = JSON.parse(localStorage.getItem("airs") || `[]`);
            this.draft.unshift(obj);
            localStorage.setItem("airs", JSON.stringify(this.draft));
            
            // this.draft.unshift(obj);
            this.form.title="";
            this.form.city="";
            this.form.content="";
      },
      handleEdit(index){
          this.form = {...this.draft[index]}
      },
      getCity(value, callback, select) {
        if (!value) {
            return;
        }

        this.$axios({
            url: "/airs/city?name=" + value,
            method: "GET"
        }).then(res => {
                const { data } = res.data;
                const newData = data.map(v => {
                    return {
                        ...v,
                        value: v.name,
                    };
                });

                switch (select) {
                case "depart":
                    this.form.city = newData[0].value;
                    break;
                default:
                    break;
                }

                callback(newData);
            });
        },
        handleDepartSelect(item) {
            this.form.city = item.value;
        },
        queryDepartSearch(value, callback) {
            this.getCity(value, callback, "depart");
        },
        // 递交表单
        handleSubmit(){
            if(!this.$store.state.user.userInfo.token){
                    this.$message.warning('请先登录')
                    this.$router.push('/user/login')
            }
            if(!this.form.title||!this.form.content||!this.form.city){
               this.$message.warning('请输入完整的信息')
               return
            }
            const {title,content,city} = {...this.form}
            const data = {title,content,city}
            const { token } = this.$store.state.user.userInfo;
            this.$axios({
                url:'/posts',
                method:'POST',
                data,
                headers: {
                    Authorization: `Bearer ${token}`
                }
            }).then(res => {
                const {status,message} = res.data;
                if(status===0){
                    this.$message.success(message)
<<<<<<< HEAD

                    this.$router.push('/post')

=======
                    this.$router.push('/post')
>>>>>>> 61f97e88bb084c4d1879a98de71430daa2704006
                }
                if(status===1){
                    this.$message.warning(message)
                }
            })
        },
        //  删除草稿
        handleDel(index){
            const obj = {...this.form};
            this.draft = JSON.parse(localStorage.getItem("airs") || `[]`);
            this.draft.splice(index,1);
            localStorage.setItem("airs", JSON.stringify(this.draft));
            this.$message.success('删除成功');
        }
    }
  }
</script>
<style scoped lang="less">
.container {
  width: 1000px;
  margin: 20px auto;
  .box-right{
      width: 200px;
      min-height: 100px;
      .draft{
        width: 200px;
        min-height: 100px;
        border: 1px solid rgba(0,0,0,0.2);
        h4{
            font-size: 18px;
            font-weight: 350;
            margin: 10px;
            color: rgba(0,0,0,0.5);
        }
        div{
            font-size: 15px;
            font-weight: 100;
            margin: 10px;
            color: rgba(0,0,0,1);
        }
        p{
            font-size: 13px;
            color: #ccc;
        }
        .draft-2{
            margin-left: 0px;
            position: relative;
            div{
                &:hover{
                    color: orange;
                    cursor: pointer;
                }
            }
            
            a{
                cursor: pointer;
                position: absolute;
                top: 0px;
                right: -5px;;
                &:hover{
                    color: red;
                }
            }
        }
      }
      
  }

  .box-left{
      flex: 1;
      margin-right: 50px;
      
      h2{
          font-size: 22px;
          font-weight: 400;
          margin-bottom: 10px;
          display: block;
      }
      p{
          margin-bottom: 10px;
          color:#999;
          font-size: 12px;
      }
      .input-title{
          margin-bottom: 16px;
      }

      .quill-editor {
          min-height: 200px;
          max-height: 200px;
          margin-bottom: 16px;
      }
      .input-city{
          width: 200px;
          margin-bottom: 16px;
      }
      .textbox{
          quill-editor{
            height: 500px;;
          }
      }
      .textbox{
          margin-bottom: 30px;
        /deep/ .ql-container{
            height: 150px;
            overflow: hidden;
        }
      }
  }
}

</style>
