<template>
  <!-- 地图 start -->
  <div id="container"></div>
  <!-- 地图 end -->
</template>

<script>
export default {
  mounted() {
    //  加载地图
    window.onLoad = function() {
      this.map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [116.397428, 39.90923], //中心点坐标
        viewMode: "3D" //使用3D视图
      });
    };
    var url = `https://webapi.amap.com/maps?v=1.4.15&key=dd4797c0de1f24ec83c5eb7b8d92ce5e&callback=onLoad`;
    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  }
};
</script>

<style lang="less" scoped>
#container {
  width: 420px;
  height: 260px;
}
</style>

