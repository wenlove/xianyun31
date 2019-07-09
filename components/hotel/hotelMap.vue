<template>
  <!-- 地图 start -->
  <div>
    <div id="container"></div>
    <input type="text" />
  </div>

  <!-- 地图 end -->
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      default: () => {}
    }
  },
  data() {
    return {
      map: null,
      marker: null,
      longitude: 116.397428, //坐标点
      latitude: 39.90923,
      city: "北京市" //城市名
    };
  },
  methods: {
    //加载地图
    getMap() {
      let location = {};
      const _this = this;
      // console.log(_this.data[0])

      if (_this.data[0]) {
        location = _this.data[0].location;
        _this.city = _this.data[0].city.name;
      }
      _this.longitude = location.longitude; //经度
      _this.latitude = location.latitude; //经度
      // console.log(_this.longitude, _this.latitude, _this.city);

      _this.map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [116.397428, 39.90923], //中心点坐标
        viewMode: "3D" //使用3D视图
      });

      //点标记
      _this.marker = new AMap.Marker({
        // content: '<div class="marker-route marker-marker-bus-from">1</div>',
        position: new AMap.LngLat(116.397428, 39.90923), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
        title: "北京市"
      });

      //// 将创建的点标记添加到已有的地图实例：
      _this.map.add([_this.marker]);
    }
  },
  mounted() {
    const _this = this;
    //  加载地图
    window.onLoad = function() {
      _this.getMap();
    };
    var url = `https://webapi.amap.com/maps?v=1.4.15&key=dd4797c0de1f24ec83c5eb7b8d92ce5e&callback=onLoad`;
    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  },
  watch: {
    $route() {
      // let location = {};

      // if (this.data[0]) {
      //   location = this.data[0].location;
      //   this.city = this.data[0].city.name;
      // } else {
      //   location = { longitude: this.longitude, latitude: this.latitude };
      // }
      // this.longitude = location.longitude; //经度
      // this.latitude = location.latitude; //经度
      // console.log(this.longitude, this.latitude, this.city);
      // const city =city.name;
      setTimeout(() => {
        this.getMap();
      }, 100);
    }
  }
};
</script>

<style lang="less" scoped>
#container {
  width: 420px;
  height: 260px;
}
</style>

