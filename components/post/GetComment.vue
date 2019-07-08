<template>
    
    <!-- 评论 -->
    <div class="post-wrapper">

        <!-- 品论显示 -->
        <div class="cmt-list">
            <!-- 具体个数待定 -->
            <div class="cmt-item" v-for="(item,index) in list" :key="index">
                <div class="cmt-info">
                    <img :src="`${$axios.defaults.baseURL }${item.account.defaultAvatar}`" alt="">
                    {{ item.account.nickname}}
                    <i>{{moment(item.created_at).format("YYYY-MM-DD hh-mm-ss")}}</i>
                    <span>1</span>
                </div>
                <div class="cmt-content">
                    <div class="cmt-new">
                        <p class="cmt-message">{{item.content}}</p>
                        
                        <div class="cmt-floor">
                            <!-- 回复 -->
                        </div>
                        <div class="cmt-img" v-if="item.pics.length">
                            <div class="cmt-imgline" v-for="(item1,index) in item.pics" :key="index">
                                <img :src="`${$axios.defaults.baseURL }${item1.url}`" alt="">
                            </div>
                        </div>
                    </div>
                    <div class="cmt-ctrl">
                        <a href="#texarea" @click="handleReply(item.id)" >回复</a>
                    </div>
                </div>
            </div>
            
        </div>

        <!-- 分页按钮 -->
        <div class="block">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="pageIndex"
              :page-sizes="[2, 4, 6, 8]"
              :page-size="pageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="total">
            </el-pagination>
        </div>
    </div>
    
</template>

<script>
import moment from "moment"
export default {
    data(){
        return{
            comments:[],//评论数据
            list:[],

            moment,

            score:[],

            isShow:true,

            total:0,//评论总条数
            pageIndex:1,//默认跳转
            pageSize:2,
               

        }
    },
    methods:{
        //切换条数时触发
        handleSizeChange(val) {
            
          this.pageSize=val
          console.log(this.pageSize,val);
          this.list=this.comments.slice(0,this.pageSize)
      },
      //切换页数时触发
      handleCurrentChange(val) {
        this.pageIndex=val
        this.list=this.comments.slice((this.pageIndex-1)*this.pageSize,(this.pageIndex*this.pageSize))

      }
    },
    handleReply(){},
    mounted(){
        const id=this.$route.query.id
        this.$axios({
            url:"/posts/comments",
            params:{
              post:id,
              _limit:20,
              _start:0,
            }
                }).then(res=>{
                    console.log(res,446632);
                    const data=res.data.data

                    this.comments=data//是一个评论数组

                    this.total=res.data.total//评论总条数

                    
                    this.list= data.slice(0,2)

                    const {user} = this.$store.state.user.userInfo;

                    this.score=user

                   
                    // this.isShow=false
                  
                })
    }
}
</script>

<style lang="less" scoped>
 .post-ctrl{
            padding: 50px 0 30px;
            .content-ctrl{
                text-align: center;
                .item-ctrl{
                margin: 0 20px;
                i{
                    font-size: 32px;
                    color: orange;
                    display: block;
                }
                span{
                    color: #999;
                    display: block;
                }
                }
            }
            
        }
        .post-wrapper{
            
            .texarea{
                padding-bottom: 10px;
                
            }
            .image-item{
                padding-bottom: 30px;
                
            }
            .cmt-list{
                border: 1px solid #999;
                padding: 0 0 5px 0;
                
                .cmt-item{
                color: #666;
                font-size: 12px;
                border-bottom: 1px dashed #999;
                padding: 20px 20px 0 20px;
                &:last-child{
                    border-bottom:none;
                }
                    .cmt-info{
                        img{
                        border-radius: 50%;
                        width: 20px;
                        vertical-align:middle;
                        }
                        i{
                            color: #999;
                            font-size: 12px;
                        }
                        span{
                            float: right;
                        }
                    }
                    .cmt-content{
                        padding-left: 30px;
                        
                        .cmt-new{
                            p{
                                margin-top: 10px;
                            }
                            .cmt-message{
                                font-size: 16px;
                                color: black;
                            }
                            .cmt-img{
                                    padding-top: 15px;
                                    .cmt-imgline{
                                        padding: 5px;
                                        border: 1px dashed #999;
                                        border-radius: 5px;
                                        display: inline-block;
                                        img{
                                            width: 60px;
                                        }
                                    }
                                }
                            
                        }
                        .cmt-ctrl{
                           
                            text-align: right;
                            a{
                                 height: 20px;
                                line-height: 20px;
                                color: #1e50a2;
                                font-size: 12px;
                                display: block;
                                padding-bottom: 5px;
                            }
                        }
                    }
                }
                
            }

            .block{
                padding-top: 20px;
                text-align: center;
            }
        }
</style>
