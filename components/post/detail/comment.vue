<template>
    
    <!-- 评论，收藏，分享，点赞， -->
    <div class="post-ctrl">
        <el-row type="flex"  class="content-ctrl" justify="center">
            <div class="item-ctrl">
                <i class="el-icon-edit-outline"></i>
                <span>评论(4)</span>
            </div>
            <div class="item-ctrl" @click="handleClickStar">
                <i class="el-icon-star-off"></i>
                <span>收藏</span>
            </div>
            <div class="item-ctrl">
                <i class="el-icon-share"></i>
                <span >分享</span>
            </div>
            <div class="item-ctrl" @click="handleClickGood">
                <i class="el-icon-thumb"></i>
                <span>点赞(1)</span>
            </div>
        </el-row>
    </div>
        
</template>

<script>

export default {
    data(){
        return{

        }
    },
    methods:{
        // 点赞，收藏...
        handleClick(url){
            const {token} = this.$store.state.user.userInfo;
            this.$axios({
                url:`/posts/${url}?id=`+this.$route.query.id,
                headers:{
                    Authorization:`bearer ${token}`,
                }
            }).then(res=>{
                    // console.log(res);
                    const{status,message}=res.data
                    if(status==0){
                        this.$message.success(message)
                    }
                    // console.log(this.comment);
                })
        },

        //   点赞
        handleClickGood(){
            this.handleClick("like");
        },
        //   收藏
        handleClickStar(){
            this.handleClick("star");
        }, 
    },
    mounted(){
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
            h4{
                font-size: 18px;
                margin-bottom: 20px;
                font-weight: 400;
            }
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
