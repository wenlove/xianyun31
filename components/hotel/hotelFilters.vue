<template>
  <div class="fiters">
    <!-- 筛选酒店 start -->
    <div class="list-filter">
      <el-row type="flex">
        <el-col class="filter-col" :span="6" style="padding:5px 20px;">
          <el-row type="flex" justify="space-tween">
            <el-col>价格</el-col>
            <el-col>0-{{slider}}</el-col>
          </el-row>
          <el-slider v-model="slider" @change="handleSlider" :max="4000"></el-slider>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>住宿等级</el-col>
          </el-row>
          <el-select
            v-model="ranksValue"
            clearable
            @clear="handleRankEmpty"
            collapse-tags
            placeholder="不限"
            @change="handleRank"
          >
            <el-option
              v-for="(item,index) in ranks"
              :key="index"
              :label="item.name"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>住宿类型</el-col>
          </el-row>
          <el-select
            v-model="typesValue"
            clearable
            @clear="handleTypesEmpty"
            collapse-tags
            @change="handleTypes"
            placeholder="不限"
          >
            <el-option
              v-for="(item,index) in types"
              :key="index"
              :label="item.name"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>酒店设施</el-col>
          </el-row>
          <el-select
            v-model="facilityValue"
            clearable
            @clear="handleFacilitysEmpty"
            collapse-tags
            @change="handleFacilitys"
            placeholder="不限"
          >
            <el-option
              v-for="(item,index) in facilitys"
              :key="index"
              :label="item.name"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>酒店品牌</el-col>
          </el-row>
          <el-select
            v-model="brandValue"
            clearable
            @clear="handleBrandsEmpty"
            collapse-tags
            @change="handleBrands"
            placeholder="不限"
          >
            <el-option
              v-for="(item,index) in brands"
              :key="index"
              :label="item.name"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-col>
      </el-row>
    </div>
    <!-- 筛选酒店 end -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      options: [
        {
          name: "选项1",
          label: "黄金糕"
        },
        {
          name: "选项2",
          label: "双皮奶"
        }
      ],
      //筛选条件
      //住宿等级
      ranks: [
        {
          value: "选项1",
          label: "黄金糕"
        }
      ],
      //住宿类型
      types: [
        {
          value: "选项1",
          label: "黄金糕"
        }
      ],
      // 设施
      facilitys: [
        {
          value: "选项1",
          label: "黄金糕"
        }
      ],
      //酒店品牌
      brands: [
        {
          value: "选项1",
          label: "黄金糕"
        }
      ],
      slider: 4000, //滑块
      ranksValue: [],
      typesValue: [],
      facilityValue: [],
      brandValue: [],

      hotelasset: 0, //酒店设施
      hotelbrand: 0, //酒店品牌
      hoteltype: 0, //酒店类型
      hotellevel: 0, //酒店星级
      price_in: 0 //酒店价格
    };
  },
  methods: {
    //价格筛选
    handleSlider(value) {
      // console.log(value);
      this.$emit("setDataList", { price_lt: value });
    },
    //住宿等级
    handleRank(value) {
      // console.log(value);

      this.$emit("setDataList", { hotellevel: value });
    },
    //清空住宿等级
    handleRankEmpty(value) {
      this.$emit("setDataList", {});
    },
    //住宿类型
    handleTypes(value) {
      // console.log(value);
      this.$emit("setDataList", {hoteltype: value});
    },
    //清空住宿类型
    handleTypesEmpty() {
      this.$emit("setDataList", {});
    },
    //设施
    handleFacilitys(value) {
      // console.log(value);
      
      this.$emit("setDataList", { hotelasset: value});
    },
    //清空设施
    handleFacilitysEmpty() {
      this.$emit("setDataList", {});
    },
    //酒店品牌
    handleBrands(value) {
      // console.log(value);
      this.$emit("setDataList", {hotelbrand: value});
    },
    //清空品牌
    handleBrandsEmpty() {
      this.$emit("setDataList", {});
    }
  },
  mounted() {
    //获取酒店筛选条件
    this.$axios({
      url: "/hotels/options"
    }).then(res => {
      const { data } = res.data;
      // console.log(data)
      if (res.status == 200) {
        this.ranks = data.levels;
        this.types = data.types;
        this.facilitys = data.assets;
        this.brands = data.brands;
      }
    });
  }
};
</script>

<style scoped lang="less">
.fiters {
  padding-top: 20px;
}
//筛选酒店
.list-filter {
  border: 1px solid #ddd;
  > div {
    padding: 5px 0;
  }
  .filter-col {
    border-right: 1px solid #ddd;
    padding: 5px 0;
    &:last-child {
      border-right: 0;
    }
    .filter-title {
      padding: 0 15px;
    }
    /deep/.el-input__inner {
      border: none;
    }
  }
}
</style>