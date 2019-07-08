<template>
  <div class="hotel">
    <div class="head">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/hotel'}">酒店</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: '/hotel?city='+city }">南京酒店</el-breadcrumb-item>
        <el-breadcrumb-item>{{dataList.name}}</el-breadcrumb-item>
        <br />
        <div class="abc">
          <h2>{{dataList.name}}</h2>
          <p>{{dataList.alias}}</p>
          <span class="el-icon-location">{{dataList.address}}</span>
        </div>
      </el-breadcrumb>
    </div>
    <!-- 图片 -->
    <div class="Displayimg">
      <div class="img1">
        <img src="@/static/1.jpeg" v-if="message===1" />
        <img src="@/static/2.jpeg" v-if="message===2" />
        <img src="@/static/3.jpeg" v-if="message===3" />
        <img src="@/static/4.jpeg" v-if="message===4" />
        <img src="@/static/5.jpeg" v-if="message===5" />
        <img src="@/static/6.jpeg" v-if="message===6" />
      </div>
      <div class="img2">
        <el-row>
          <div class="grid-conten4" offset="7">
            <img src="@/static/1.jpeg" @click="handleChange(arr[0])" />
            <img src="@/static/2.jpeg" @click="handleChange(arr[1])" />
            <img src="@/static/3.jpeg" @click="handleChange(arr[2])" />
            <img src="@/static/1.jpeg" @click="handleChange(arr[3])" />
            <img src="@/static/2.jpeg" @click="handleChange(arr[4])" />
            <img src="@/static/3.jpeg" @click="handleChange(arr[5])" />
          </div>
        </el-row>
      </div>
    </div>
    <!-- 价格来源  低价房型-->
    <div class="information">
      <el-row type="flex" class="row-bg1" justify="space-around">
        <el-col :span="8">
          <div class="grid-content bg-purple">价格来源</div>
        </el-col>
        <el-col :span="8">
          <div class="grid-content bg-purple-light">低价房型</div>
        </el-col>
        <el-col :span="8">
          <div class="grid-content bg-purple">最低价格/每晚</div>
        </el-col>
      </el-row>

      <el-row
        type="flex"
        class="row-bg"
        justify="space-around"
        v-for="(item,index) in products"
        :key="index"
      >
        <el-col :span="8">
          <a href="https://hotels.ctrip.com/hotel/694679.html">
            <div class="grid-content bg-purple">{{item.name}}</div>
          </a>
        </el-col>
        <el-col :span="8">
          <a href="https://hotels.ctrip.com/hotel/694679.html">
            <div class="grid-content bg-purple-light">{{item.bestType}}</div>
          </a>
        </el-col>
        <el-col :span="8">
          <a href="https://hotels.ctrip.com/hotel/694679.html">
            <div class="grid">
              ￥{{item.price}}
              <span>起</span> >
            </div>
          </a>
        </el-col>
      </el-row>
    </div>
    <!-- 地图 -->
    <div class="map">
      <div id="container"></div>
      <div class="tab">
        <el-tabs @tab-click="handleBoom">
          <el-tab-pane label="风景"></el-tab-pane>
          <el-tab-pane label="交通"></el-tab-pane>
        </el-tabs>
        <div>
          <el-row
            type="flex"
            justify="space-between"
            v-for="(item,index) in ditu"
            :key="index"
            class="ditu"
          >
            <span>{{item}}</span>
            <p>1.03公里</p>
          </el-row>
        </div>
      </div>
    </div>
    <!-- 基本信息 -->
    <div class="hotelservices">
      <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="date" label="基本信息" width="180"></el-table-column>
        <el-table-column prop="name" label="入住时间:14:00之后" width="180"></el-table-column>
        <el-table-column prop="address" label="离店时间: 12:00之前"></el-table-column>
        <el-table-column prop="address" label="2010年开业 / 2015年装修"></el-table-column>
        <el-table-column prop="address" label="酒店规模: 148间客房"></el-table-column>
      </el-table>
    </div>

    <!-- 评分 -->
    <div class="grade">
      <h4>四条真实在用户评论</h4>
      <!-- 四条真是用户评论 -->
      <el-row type="flex" align="middle">
        <el-col :span="5">
          <div class="pinglun">
            <ul>
              <li>总评数:{{common_remarks}}</li>
              <li>好评数:{{good_remarks}}</li>
              <li>差评数:{{common_remarks-good_remarks}}</li>
            </ul>
          </div>
        </el-col>
        <el-col :span="19">
          <el-row type="flex">
            <el-col :span="6">
              <!-- 星星评分 -->
              <div class="pingfen">
                <el-rate
                  v-model="value"
                  disabled
                  show-score
                  text-color="#ff9900"
                  score-template="{value}"
                ></el-rate>
              </div>
            </el-col>
            <el-col :span="6">
              <!-- 环境 -->
              <el-row type="flex" class="huanjing">
                <el-col>环境</el-col>
                <el-col>产品</el-col>
                <el-col>服务</el-col>
              </el-row>
            </el-col>
            <el-col :span="6"></el-col>
            <el-col :span="6"></el-col>
          </el-row>
        </el-col>
      </el-row>
    </div>

    <!-- 用户评论区域 -->
    <div class="comment">
      <div class="pingluninput">
        <ul v-for="(item,index) in hotel.content" :key="index">
          <li>
            <div class="wenzi">{{item}}</div>
            <br />
            <el-row type="flex" justify="space-between">
              <el-row class="boom" type="flex" direction="vertical" align="center">
                <img src="@/static/7.jpeg" />
                <p>2019-6-26</p>
              </el-row>
              <el-input placeholder="添加回复">
                <span></span>
              </el-input>
              <p>
                <el-button type="primary">回复</el-button>
              </p>
            </el-row>
            <ul class="asd" v-for="(v,i) in hotel.followed.content" :key="i">
              <li>
                <div class="wenzi">{{v}}</div>
                <el-row type="flex" justify="space-between">
                  <el-row class="boom" type="flex" direction="vertical" align="center">
                    <img src="@/static/7.jpeg" />
                    <p>2019-6-26</p>
                  </el-row>
                  <el-input placeholder="添加回复"></el-input>
                </el-row>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city:"",
      hotel: {
        content: [],
        followed: { content: [] }
      },
      common_remarks: 0,
      good_remarks: 0,
      ditu: [
        "高淳老街",
        "红太阳广场",
        "人民广场",
        "宝塔公园",
        "保圣祠",
        "春东湖广场",
        "保圣祠公园",
        "红红的太阳",
        "固城湖水慢城",
        "绿地广场"
      ],
      //请求回来的总数据
      dataList: [],

      //价格来源的数据
      products: [],

      //酒店基本信息
      hotelassets: [],
      //星星评分
      hotellevel: {},
      arr: [1, 2, 3, 4, 5, 6],
      message: 1,
      //地图右面模块
      editableTabs: [
        {
          title1: "风景",
          title2: "交通"
        }
      ],

      // 价格来源

      //基本信息
      tableData: [
        {
          date: "主要设施",
          name: "外币兑换服务 电梯 洗衣服务 热水壶"
        },
        {
          date: "停车服务",
          name: "-"
        },
        {
          date: "品牌信息",
          name: "-"
        }
      ],
      //评分
      value: 3.5
    };
  },
  mounted() {
    this.handleWindow();

    const id = this.$route.query.id;
    this.$axios({
      url: "/hotels?id="+id,
      method: "GET",
      
    }).then(res => {
      // console.log(res.data);
      const { data } = res.data;
      console.log(data[0])
      this.dataList = data[0];
      this.city = data[0].city.id;
      this.products = data[0].products;
      this.hotelassets = data[0].hotelassets;
      this.value = res.data.data[0].hotellevel.level;
      this.common_remarks = res.data.data[0].common_remarks;
      this.good_remarks = res.data.data[0].good_remarks;
    });

    this.$axios({
      url: "/comments",
      data: {
        id
      }
    }).then(res => {
      // console.log(res.data);
      this.hotel.content.push(res.data[0].content);
      this.hotel.followed.content.push(res.data[0].followed[0].content);
    });
  },
  methods: {
    handleChange(index) {
      this.message = index;
    },
    handleBoom() {
      this.ditu = this.ditu.reverse();
    },

    handleWindow() {
      //  地图
      window.onLoad = function() {
        var map = new AMap.Map("container", {
          zoom: 11, //级别
          center: [116.397428, 39.90923], //中心点坐标
          viewMode: "3D" //使用3D视图
        });
        //控制条和缩放的工具
        var toolbar = new AMap.ToolBar();
        map.addControl(toolbar);

        //点标记
        // 创建一个 Marker 实例：
        var marker = new AMap.Marker({
          content: '<div class="marker-route marker-marker-bus-from">1</div>',
          position: new AMap.LngLat(118.87, 31.328), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
          title: "高淳县淳溪镇"
        });
        // 将创建的点标记添加到已有的地图实例：
        map.add(marker);
      };
      var url = `https://webapi.amap.com/maps?v=1.4.15&key=d6d10158cfd6993d44054ed6a27cce49&callback=onLoad&plugin=AMap.ToolBar`;
      var jsapi = document.createElement("script");
      jsapi.charset = "utf-8";
      jsapi.src = url;
      document.head.appendChild(jsapi);
    }
  }
};
</script>

<style scoped lang="less">
.ditu {
  margin-bottom: 10px;
  font-size: 15px;
}
.hotel {
  width: 1000px;
  margin: 0 auto;
}
.grid-conten4 {
  width: 100%;
}
.grid-conten4 img {
  width: 160px;
  height: 110px;
  margin-bottom: 10px;
  margin-left: 6px;
}
.head {
  width: 100%;
}
.head p {
  margin-top: 5px;
}
.abc {
  margin-top: 20px;
}
.abc p {
  width: 100%;
}
.Displayimg {
  display: flex;
  margin-top: 30px;
}
.img1 img {
  display: block;
  padding-right: 20px;
  width: 640px;
  height: 360px;
}
.img2 {
  width: 343px;
  height: 360px;
}
.information {
  width: 100%;
  margin-top: 60px;
}
.information ul {
  display: flex;
  margin-left: 5px;
  /* height: 48px; */
  /* line-height: 48px; */
}
.information ul li {
  width: 33%;
  font-size: 15px;
}
.information ul li span {
  padding-left: 92px;
}
.information li p {
  display: block;
  margin-left: 92px;
}
.map {
  display: flex;
}
.hotelservices {
  margin-bottom: 40px;
}
#container {
  width: 650px;
  height: 360px;
}
.tab {
  width: 330px;
  padding-left: 20px;
}
.tab li {
  padding-right: 100px;
}
.grade {
  /* display: flex; */
}
.grade h4 {
  margin-bottom: 20px;
}
.huanjing {
  color: coral;
}
.comment img {
  width: 44px;
  height: 44px;
  border: 1px solid coral;
  border-radius: 50%;
}
.comment {
  display: flex;
}
.pingluninput {
  flex: 1;
  margin-left: 20px;
}
.asd {
  margin: 20px 0 20px 50px;
}
p {
  width: 60px;
  font-size: 5px;
}
.boom {
  margin-right: 20px;
  flex-direction: column;
}
.wenzi {
  margin-left: 80px;
}
.grid {
  color: coral;
}
.grid span {
  color: black;
}
.row-bg {
  font-size: 15px;
  padding-bottom: 20px;
  padding-top: 20px;
}
.row-bg1 {
  border-bottom: 1px solid #ccc;
  padding-bottom: 20px;
}
.row-bg {
  border-bottom: 1px solid #ccc;
  margin-bottom: 20px;
}
</style>
