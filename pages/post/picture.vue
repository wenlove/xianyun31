<template>
    <div class="container">
        <div class="list" v-for="(item,index) in list" :key="index">
            <div :style="{position:`fixed`,
                          width:`20px`,
                          height:`20px`,
                          borderRadius:'50%',
                          backgroundColor:`red`,
                          left:`${left[index]}px`,
                          top:`${top[index]}px`}"></div>
    </div>
            <div :style="{position:`fixed`,
                          width:`20px`,
                          height:`20px`,
                          backgroundColor:`red`,
                          left:`${aa}px`,
                          top:`${aa}px`}"></div>
        </div>
</template>

<script>
export default {
    data(){
        return {
            list:[],
            left:[],
            top:[],
            aa:0,
        }
    },
    methods:{
        handleadd(){
            
        }
    },
    mounted(){
        for(var j=0;j<20;j++){
            this.list.push(0);
            this.left.push(0);
            this.top.push(0);
        }
        var timer = setInterval(()=>{
            for(var i=0;i<this.list.length;i++){
                this.left[i]+=Math.random()
                this.top[i]+=Math.random()
                this.aa+=1;
                // console.log(this.left,this.top);
            }
        },10)
    }
}
</script>

<style scoped lang="less">
.container {
  width: 1000px;
  height: 1000px;
  margin: 20px auto;
    .ball{
        // position: fixed;
        width: 300px;
        height: 300px;
        background-color: red;
        border-radius: 50%;
        position: fixed;
    }

}
</style>
