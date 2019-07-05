<template>
  <div>
    <h1>高德地图</h1>
    <div id="container"></div>
    <div id="panel"></div>
    <div>
      <select v-model="city">
        <option value="广州">广州</option>
      </select>
      <input type="text" v-model="start" placeholder="起始位置" />
      <input type="text" v-model="end" placeholder="终点位置" />
      <button @click="handleSearch">驾车</button>
      <button @click="handleTransfer">公交</button>
      <button @click="handleWalking">步行</button>
      <button @click="handleRiding">骑行</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      start: "",
      end: "",
      map: null
    };
  },
  methods: {
    //获取乘车路线
    getWay() {
      this.map = new AMap.Map("container", {
        zoom: 11 //级别
      });
    },
    //驾车乘车路线
    handleSearch() {
      this.map = new AMap.Map("container", {
        zoom: 11 //级别
      });
      //查询驾车路线
      var driving = new AMap.Driving({
        // 驾车路线规划策略，AMap.DrivingPolicy.LEAST_TIME是最快捷模式
        policy: AMap.DrivingPolicy.LEAST_TIME,
        // map 指定将路线规划方案绘制到对应的AMap.Map对象上
        map: this.map,
        // panel 指定将结构化的路线详情数据显示的对应的DOM上，传入值需是DOM的ID
        panel: "panel",
        city: this.city
      });

      var points = [
        { keyword: this.start, city: this.city },
        { keyword: this.end, city: this.city }
      ];

      driving.search(points, function(status, result) {
        // 未出错时，result即是对应的路线规划方案
      });
    },
    //公交乘车路线
    handleTransfer() {
      this.map = new AMap.Map("container", {
        zoom: 11 //级别
      });
      //公交乘车路线，构造公交换乘类
      var transfer = new AMap.Transfer({
        map: this.map,
        city: this.city,
        panel: "panel",
        policy: AMap.TransferPolicy.LEAST_TIME //乘车策略
      });

      var points = [
        { keyword: this.start, city: this.city },
        { keyword: this.end, city: this.city }
      ];
      //根据起、终点名称查询公交换乘路线
      transfer.search(points, function(status, result) {
        // result即是对应的公交路线数据信息，
        console.log(result);
      });
    },
    //步行
    handleWalking() {
      this.map = new AMap.Map("container", {
        zoom: 11 //级别
      });
      //步行导航
      var walking = new AMap.Walking({
        map: this.map,
        panel: "panel"
      });

      var points = [
        { keyword: this.start, city: this.city },
        { keyword: this.end, city: this.city }
      ];
      walking.search(points,function(status, result) {
          // result即是对应的步行路线数据信息
        }
      );
    },
    //骑行
    handleRiding() {
      this.map = new AMap.Map("container", {
        zoom: 11 //级别
      });
      //步行导航
      var riding = new AMap.Riding({
        map: this.map,
        panel: "panel"
      });

      var points = [
        { keyword: this.start, city: this.city },
        { keyword: this.end, city: this.city }
      ];
      riding.search(points,function(status, result) {
          // result即是对应的步行路线数据信息
        }
      );
    }
  },
  mounted() {
    window.onLoad = function() {
      this.map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [116.397428, 39.90923], //中心点坐标
        viewMode: "3D" //使用3D视图
      });

      //工具条插件
      var toolbar = new AMap.ToolBar();
      map.addControl(toolbar);

      //点标记
      // var marker = new AMap.Marker({
      //   position: new AMap.LngLat(116.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
      //   title: "北京"
      // });
      // var marker1 = new AMap.Marker({
      //   position: new AMap.LngLat(116.29, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
      //   title: "北京"
      // });

      //// 将创建的点标记添加到已有的地图实例：
      // map.add([marker, marker1]);

      //拖拽导航
      // path 是驾车导航的起、途径和终点，最多支持16个途经点
      //   var path = [];
      //   path.push([116.303843, 39.983412]);
      //   path.push([116.321354, 39.896436]);
      //   path.push([116.407012, 39.992093]);

      //   var route = new AMap.DragRoute(map, path, AMap.DrivingPolicy.LEAST_FEE);
      //   // 查询导航路径并开启拖拽导航
      //   route.search();
    };

    var str =
      "AMap.ToolBar,AMap.Driving,AMap.DragRoute,AMap.Transfer,AMap.Walking,AMap.Riding";

    var url = `https://webapi.amap.com/maps?v=1.4.15&key=dd4797c0de1f24ec83c5eb7b8d92ce5e&callback=onLoad&plugin=${str}`;
    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  }
};
</script>

<style scoped>
#container {
  width: 500px;
  height: 500px;
}

#panel {
  position: fixed;
  background-color: white;
  max-height: 90%;
  overflow-y: auto;
  top: 10px;
  right: 10px;
  width: 280px;
}
#panel .amap-call {
  background-color: #009cf9;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
}
#panel .amap-lib-driving {
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
  overflow: hidden;
}
</style>
