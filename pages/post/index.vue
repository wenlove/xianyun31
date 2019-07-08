<template>
    <div class="container">
        <h1>旅游首页</h1>
        <p>跳转页面的时候要把ID传过来</p>
        <nuxt-link to="post/detail?id=4">塞班岛</nuxt-link>
        <nuxt-link to="post/detail?id=5">远东国</nuxt-link>
        <nuxt-link to="post/detail?id=6">成都市</nuxt-link>

        </div>
</template>

<script>
export default {
 data() {
      return {
       
      };
    },
    methods: {
      


    

    }
}
</script>

<style>
.container{
    width: 800px;
    margin: 50px auto
}
</style>
