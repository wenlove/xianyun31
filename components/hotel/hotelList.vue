<template>
  <div class="hotel-list">
    <el-row type="flex" class="hotel-item" v-for="(item,index) in data" :key="index">
      <el-col :span="8">
        <a :href="`/hotel/hotelDetail?id=${item.id}`" class="img-wrapper">
          <!-- <img
            src="http://qcloud.dpfile.com/pc/wQ2ejOSmhB4OoEq1LYPHp_2RhNciyW026YiQFr_EEZsQrI_ZB6abZPwgKDOmI03U3dWYFEM5hoslgWXTJDjSPA.jpg"
            alt
          />-->
          <img :src="item.photos" alt />
        </a>
      </el-col>
      <el-col :span="10">
        <h4 class="hotel-name">
          <a :href="`/hotel/hotelDetail?id=${item.id}`">{{item.name}}</a>
        </h4>
        <!-- 酒店信息 start -->
        <el-row class="hotel-en-name">
          <span>{{item.alias}}</span>
          <span :title="`${item.hotellevel?item.hotellevel.name+'级':'0星级'}`">
            <i
              class="iconfont iconhuangguan"
              v-for="item1 in (item.hotellevel?item.hotellevel.level:0)"
              :key="item1"
            ></i>
          </span>
          <span>{{item.hoteltype.name}}</span>
        </el-row>
        <!-- 酒店信息 end -->
        <!-- 评价相关 start -->
        <el-row type="flex" class="hotel-evaluate">
          <el-col :span="11">
            <el-rate
              v-model="item.stars"
              disabled
              show-score
              text-color="#ff9900"
              score-template="{value}分"
            ></el-rate>
          </el-col>
          <el-col :span="7">
            <span class="height-light">{{item.all_remarks}}</span>
            条评价
          </el-col>
          <el-col :span="7">
            <span class="height-light">79</span>
            篇游记
          </el-col>
        </el-row>
        <!-- 评价相关 end -->
        <!-- 摘要 start -->
        <div class="hotel-summary-row"></div>
        <!-- 摘要 end -->
        <div class="location-row">
          <i class="iconfont iconlocation"></i>
          位于: {{item.address}}
        </div>
      </el-col>
      <!-- 酒店价格 start -->
      <el-col :span="6" class="price-col">
        <el-row
          type="flex"
          class="price-item"
          align="middle"
          justify="space-btween"
          v-for="(item1,index1) in item.products"
          :key="index1"
        >
          <el-col>{{item1.name}}</el-col>
          <el-col :span="14">
            <span class="height-light hotel-sell-price">￥{{item1.price}}</span>起
            <i class="el-icon-arrow-right"></i>
          </el-col>
        </el-row>
      </el-col>
      <!-- 酒店价格 end -->
    </el-row>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      default: ()=>[]
    }
  },
  data() {
    return {
      value: 3.5 //评分
    };
  }
};
</script>

<style lang="less" scoped>
.hotel-item {
  border-bottom: 1px solid #ddd;
  padding: 25px 0;
  .img-wrapper {
    display: block;
    background: #ccc;
    width: 320px;
    height: 210px;
    overflow: hidden;
  }
  .hotel-name {
    font-weight: 400;
    font-size: 24px;
  }
  .hotel-en-name {
    color: #999;
    margin-bottom: 5px;
  }
  .hotel-evaluate {
    font-size: 16px;
  }
  .location-row {
    padding-top: 10px;
  }
  .price-col {
    padding: 20px 10px 0;
    .price-item {
      height: 44px;
      border-bottom: 1px solid #ddd;
      cursor: pointer;
    }
  }
}
.iconhuangguan,
.height-light {
  color: #f90;
}
</style>

