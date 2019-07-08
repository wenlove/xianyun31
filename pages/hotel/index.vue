<template>
  <div class="main">
    <!-- 面包屑 -->
    <div class="breadcrumb">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item>酒店</el-breadcrumb-item>
        <el-breadcrumb-item>{{cityName}}酒店预订</el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <!-- 选择城市，时间，人数过滤条件 -->
    <HotelSelect @setCity="setCity" />

    <!-- 描叙与地图 start -->
    <el-row class="hotel-option-row" type="flex">
      <el-col :span="14" style="padding-left: 5px; padding-right: 5px;">
        <!-- 区域 start -->
        <el-row type="flex" class="option-row">
          <el-col :span="3">区域:</el-col>
          <el-col :span="21" class="hidden-all">
            <div class="scenics-box">
              <a href>全部</a>
              <a
                href="#"
                v-for="(item,index) in scenic"
                :key="index"
                class="location-budget"
              >{{item.name}}</a>
            </div>
            <a href="javascript:;">
              <i class="el-icon-d-arrow-right"></i>
              <span>等{{scenic.length}}个区域</span>
            </a>
          </el-col>
        </el-row>
        <!-- 区域 end -->
        <!-- 攻略 start -->
        <el-row type="flex" class="option-row">
          <el-col :span="3">攻略:</el-col>
          <el-col
            :span="21"
          >北京，你想要的都能在这找到。博古通今，兼容并蓄，天下一城，如是帝都。 景点以故宫为中心向四处辐射；地铁便宜通畅，而且覆盖绝大多数景点。 由于早上有天安门升旗仪式，所以大多数人选择在天安门附近住宿。</el-col>
        </el-row>
        <!-- 攻略 end -->

        <!-- 均价 start -->
        <el-row type="flex" class="option-row price-row">
          <el-col :span="3" class="price-col">
            均价 :
            <el-tooltip
              class="item question-mark"
              effect="dark"
              content="等级均价由平日价格计算得出，节假日价格会有上浮。"
              placement="top-start"
            >
              <span>?</span>
            </el-tooltip>
          </el-col>
          <el-col :span="21">
            <el-row type="flex">
              <el-col>
                <i class="iconfont iconhuangguan" v-for="item in 3" :key="item"></i>
                <el-tooltip
                  class="item"
                  effect="dark"
                  content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                  placement="bottom"
                >
                  <span class="price-budget">¥332</span>
                </el-tooltip>
              </el-col>
              <el-col>
                <i class="iconfont iconhuangguan" v-for="item in 4" :key="item"></i>
                <el-tooltip
                  class="item"
                  effect="dark"
                  content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                  placement="bottom"
                >
                  <span class="price-budget">¥521</span>
                </el-tooltip>
              </el-col>
              <el-col>
                <i class="iconfont iconhuangguan" v-for="item in 5" :key="item"></i>
                <el-tooltip
                  class="item"
                  effect="dark"
                  content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                  placement="bottom"
                >
                  <span class="price-budget">¥768</span>
                </el-tooltip>
              </el-col>
            </el-row>
          </el-col>
        </el-row>
        <!-- 均价 end -->
      </el-col>
      <!-- 地图 start -->
      <el-col :span="10" class="hotel-map">
        <HotelMap :data="hotels" />
      </el-col>
      <!-- 地图 end -->
    </el-row>
    <!-- 描叙与地图 end -->

    <!-- 筛选酒店 start -->
    <HotelFilters @setDataList="setDataList" />
    <!-- 筛选酒店 end -->

    <!-- 酒店列表 start -->
    <HotelList :data="hotels" />
    <!-- 分页 start -->
    <el-row class="hotel-page" justify="end">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        background
        :pager-count="5"
        layout="prev, pager, next"
        :total="total"
      ></el-pagination>
    </el-row>
    <!-- 分页 end -->
    <!-- 酒店列表 end -->
  </div>
</template>

<script>
import HotelSelect from "@/components/hotel/hotelSelect.vue";
import HotelFilters from "@/components/hotel/hotelFilters.vue";
import HotelList from "@/components/hotel/hotelList.vue";
import HotelMap from "@/components/hotel/hotelMap.vue";
import HotelDetail from "@/components/hotel/hoteldetail.vue";

export default {
  components: {
    HotelSelect,
    HotelMap,
    HotelFilters,
    HotelDetail,
    HotelList
  },
  data() {
    return {
      //区域
      scenic: [],
      //酒店列表数据
      hotels: [],
      //分页数据
      dataList: [],
      //分页
      pageSize: 10,
      pageIndex: 1,
      total: 0,
      city: "", //城市id
      cityName:""//城市名称
    };
  },
  mounted() {
    
    this.getData();
  },
  methods: {
    //筛选酒店数据
    setDataList(obj) {
      // console.log('setDataList',obj);
      // console.log(this.$route.query)
      let query = { ...this.$route.query, ...obj };
      const { price_lt, hotellevel, hoteltype, hotelasset, hotelbrand } = query;

      
      if (price_lt == 0) {
        delete query.price_lt;
      }
      if (!hotellevel) {
        delete query.hotellevel;
      }
      if (!hoteltype) {
        delete query.hoteltype;
      }
      if (!hotelasset) {
        delete query.hotelasset;
      }
      if (!hotelbrand) {
        delete query.hotelbrand;
      }

      // console.log(query);
      this.$router.push({
        path: "/hotel",
        query
      });
    },
    //获取酒店列表数据
    getData() {
      // this.$router.push('/hotel?city='+this.city);
      this.$axios({
        url: `/hotels`,
        params: {
          _limit: this.pageSize,
          _start: (this.pageIndex - 1) * this.pageSize,
          ...this.$route.query
        }
      }).then(res => {
        // console.log(res.data);
        const { data, total } = res.data;

        if (res.status === 200) {
          this.hotels = data;
          //区域
          this.scenic = data[0] ? data[0].scenic : [];

          //总页数
          this.total = total;
          this.pageIndex = 1;
        }
      });
    },
    //切换分页
    handleSizeChange(val) {
      // console.log(`每页 ${val} 条`);
      this.pageSize = val;
      this.getData();
    },
    //当前页
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
      this.pageIndex = val;
      this.getData();
    },
    //筛选城市酒店数据
    setCity(value,name,content) {
      // console.log('seCity:',value,name)
      this.cityName = name;
      const query = { ...this.$route.query,city: value };
      // console.log(query)
      this.$router.push({
        path: "/hotel",
        query
      });
    }
  },
  watch: {
    $route() {
      setTimeout(() => {
        this.getData();
      }, 10);
    }
  }
};
</script>


<style lang="less" scoped>
.main {
  width: 1000px;
  margin: 0 auto;
  color: #666;
  font-size: 14px;
  padding-bottom: 200px;
  .breadcrumb {
    padding: 20px 0;
  }
}

// 描叙与地图
.hotel-option-row {
  //   background-color: #f5f5f5;
  .option-row {
    margin-bottom: 20px;
    .location-budget {
      margin-right: 1em;
      padding: 0 2px;
      border-radius: 4px;
      display: inline-block;
      cursor: pointer;
    }
    .el-icon-d-arrow-right {
      -webkit-transform: rotate(270deg);
      transform: rotate(270deg);
      color: #f90;
    }

    .hidden-all {
      .scenics-box {
        max-height: 2.8em;
        overflow: hidden;
      }
      .el-icon-d-arrow-right {
        -webkit-transform: rotate(90deg);
        transform: rotate(90deg);
      }
    }
  }
  .price-row {
    .price-col {
      padding-right: 15px;
      position: relative;
    }
    .question-mark {
      background-color: #ccc;
      color: #fff;
      display: inline-block;
      width: 14px;
      height: 14px;
      text-align: center;
      border-radius: 100%;
      font-size: 12px;
      position: absolute;
      right: 15px;
      top: 0;
    }

    .price-budget {
      margin-left: 10px;
    }
  }
  .hotel-map {
    overflow: hidden;
  }
}

.iconhuangguan {
  color: #f90;
}
// 分页
.hotel-page {
  padding: 20px 0 40px;
  .el-pagination {
    display: flex;
    justify-content: flex-end;
  }
}
</style>
