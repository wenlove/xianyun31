<template>
  <div class="hotel-list">
    <el-row type="flex" class="hotel-item">
      <el-col :span="8">
        <a href="#" class="img-wrapper">
          <img
            src="http://qcloud.dpfile.com/pc/wQ2ejOSmhB4OoEq1LYPHp_2RhNciyW026YiQFr_EEZsQrI_ZB6abZPwgKDOmI03U3dWYFEM5hoslgWXTJDjSPA.jpg"
            alt
          />
        </a>
      </el-col>
      <el-col :span="10">
        <h4 class="hotel-name">
          <a href="#">得月楼饭店</a>
        </h4>
        <!-- 酒店信息 start -->
        <el-row class="hotel-en-name">
          <span>de yue lou hotel</span>
          <span title="3星级">
            <i class="iconfont iconhuangguan"></i>
            <i class="iconfont iconhuangguan"></i>
            <i class="iconfont iconhuangguan"></i>
          </span>
          <span>经济型</span>
        </el-row>
        <!-- 酒店信息 end -->
        <!-- 评价相关 start -->
        <el-row type="flex" class="hotel-evaluate">
          <el-col :span="11">
            <el-rate
              v-model="value"
              disabled
              show-score
              text-color="#ff9900"
              score-template="{value}分"
            ></el-rate>
          </el-col>
          <el-col :span="7">
            <span class="height-light">79</span>
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
          <i class="iconfont iconlocation"></i>位于: 高淳县淳溪镇镇兴路118号(高淳县委党校对面)
        </div>
      </el-col>
      <!-- 酒店价格 start -->
      <el-col :span="6" class="price-col">
        <el-row type="flex" justify="space-btween">
          <el-col>携程</el-col>
          <el-col>
            <span class="height-light hotel-sell-price">￥51</span>起
            <i class="el-icon-arrow-right"></i>
          </el-col>
        </el-row>
      </el-col>
      <!-- 酒店价格 end -->
    </el-row>

    <!-- 分页 start -->
    <el-row class="hotel-page" justify="end">
      <el-pagination background  :pager-count="5" layout="prev, pager, next" :total="100"></el-pagination>
    </el-row>
    <!-- 分页 end -->
  </div>
</template>

<script>
export default {
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
  }
}
.iconhuangguan,
.height-light {
  color: #f90;
}
.hotel-page {
    padding: 20px 0 40px;
    .el-pagination{
        display: flex;
        justify-content: flex-end;
    }
}
</style>

