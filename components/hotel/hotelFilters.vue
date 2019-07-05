<template>
  <div class="fiters">
    <!-- 筛选酒店 start -->
    <div class="list-filter">
      <el-row type="flex">
        <el-col class="filter-col" :span="6" style="padding:5px 20px;">
          <el-row type="flex" justify="space-tween">
            <el-col>价格</el-col>
            <el-col>0-4000</el-col>
          </el-row>
          <el-slider v-model="slider" :max="4000"></el-slider>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>住宿等级</el-col>
          </el-row>
          <el-select v-model="value2" multiple collapse-tags placeholder="不限">
            <el-option
              v-for="item in select2"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>住宿类型</el-col>
          </el-row>
          <el-select v-model="value3" multiple collapse-tags placeholder="不限">
            <el-option
              v-for="item in select3"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>酒店设施</el-col>
          </el-row>
          <el-select v-model="value4" multiple collapse-tags placeholder="不限">
            <el-option
              v-for="item in select4"
              :key="item.name"
              :label="item.label"
              :value="item.name"
            ></el-option>
          </el-select>
        </el-col>
        <el-col class="filter-col" :span="6">
          <el-row type="flex" class="filter-title">
            <el-col>酒店品牌</el-col>
          </el-row>
          <el-select v-model="value5" multiple collapse-tags placeholder="不限">
            <el-option
              v-for="item in select5"
              :key="item.name"
              :label="item.label"
              :value="item.name"
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
      slider: 0, //滑块
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
      select1: [
        {
          value: "选项1",
          label: "黄金糕"
        },
        {
          value: "选项2",
          label: "双皮奶"
        }
      ],
      select2: [
        {
          value: "选项1",
          label: "黄金糕"
        },
        {
          value: "选项2",
          label: "双皮奶"
        }
      ],
      select3: [
        {
          value: "选项1",
          label: "黄金糕"
        },
        {
          value: "选项2",
          label: "双皮奶"
        }
      ],
      select4: [
        {
          value: "选项1",
          label: "黄金糕"
        },
        {
          value: "选项2",
          label: "双皮奶"
        }
      ],
      select5: [
        {
          value: "选项1",
          label: "黄金糕"
        },
        {
          value: "选项2",
          label: "双皮奶"
        }
      ],
      value1: [],
      value2: [],
      value3: [],
      value4: [],
      value5: []
    };
  },
  methods: {
    //表单提交
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //选择城市
    handleSelect() {
      console.log(11);
    },
    querySearchCity(queryString, cb) {
      //cd回调函数
    },
    //选择人数
    handleSelectNum() {
      console.log(11);
    },
    querySearchNum(queryString, cb) {
      console.log(queryString);
    }
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
      border-right: 0 ;
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