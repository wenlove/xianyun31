<template>
  <div class="main">
    <!-- 面包屑 -->
    <div class="breadcrumb">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item>酒店</el-breadcrumb-item>
        <el-breadcrumb-item>南京市酒店预订</el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <!-- 选择城市，时间，人数过滤条件 -->
    <HotelSelect />

    <!-- 描叙与地图 start -->
    <el-row class="hotel-option-row" type="flex">
      <el-col :span="14" style="padding-left: 5px; padding-right: 5px;">
        <!-- 区域 start -->
        <el-row type="flex" class="option-row">
          <el-col :span="3">区域:</el-col>
          <el-col :span="21" class="hidden-all">
            <div class="scenics-box">
              <a href="#" v-for="(item,index) in 20" :key="index" class="location-budget">全部</a>
            </div>
            <a href="javascript:;">
              <i class="el-icon-d-arrow-right"></i>
              <span>等43个区域</span>
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
        <HotelMap />
      </el-col>
      <!-- 地图 end -->
    </el-row>
    <!-- 描叙与地图 end -->

    <!-- 筛选酒店 start -->
    <HotelFilters />
    <!-- 筛选酒店 end -->

    <!-- 酒店列表 start -->
    <HotelList />
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
    HotelList,
    HotelDetail
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
</style>
