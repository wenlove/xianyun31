<template>
  <div class="fiters">
    <!-- 选择城市，时间，人数过滤 -->
    <el-form
      :inline="true"
      :model="ruleForm"
      ref="ruleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <!-- 目的地 -->
      <el-form-item prop="name">
        <el-autocomplete
          v-model="ruleForm.city"
          :fetch-suggestions="querySearchCity"
          placeholder="目的地"
          @select="handleSelectCity"
        ></el-autocomplete>
      </el-form-item>

      <!-- 选择日期 -->
      <el-form-item required>
        <el-col :span="10">
          <el-date-picker
            v-model="ruleForm.date"
            type="daterange"
            range-separator="-"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
          ></el-date-picker>
        </el-col>
      </el-form-item>

      <!-- 人数未定 -->
      <el-form-item class="select_num">
        <el-col>
          <el-input placeholder="人数未定" suffix-icon="el-icon-user" v-model="ruleForm.num" readonly></el-input>
          <div class="num el-popover el-popper">
            <el-row type="flex" align="middle" :gutter="10">
              <el-col :span="4">
                <div class="grid-content">每间</div>
              </el-col>
              <el-col :span="4"></el-col>
              <el-col :span="8">
                <el-select size="mini" v-model="ruleForm.value" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.name"
                    :label="item.label"
                    :value="item.name"
                  ></el-option>
                </el-select>
              </el-col>
              <el-col :span="8">
                <el-select size="mini" v-model="ruleForm.value" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.name"
                    :label="item.label"
                    :value="item.name"
                  ></el-option>
                </el-select>
              </el-col>
            </el-row>
            <el-row class="btn" cla type="flex" align="middle" justify="end">
              <el-button type="primary" size="mini">确定</el-button>
            </el-row>
          </div>
        </el-col>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">查看价格</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {
        name: "",
        city: "", //城市
        num: "", //人数
        date: "" //日期选择
      },
      options: [
        {
          name: "选项1",
          label: "黄金糕"
        },
        {
          name: "选项2",
          label: "双皮奶"
        }
      ]
    };
  },
  methods: {
    //获取城市数据
    getCity(queryString, cb) {
      if (!queryString) {
        return;
      }
      this.$axios({
        url: "/cities?name=" + queryString
      }).then(res => {
        // console.log(res);
        const { data } = res.data;
        console.log(data);
        // this.ruleForm.city=data[0].name;
        cb(data);
      });
    },
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
    handleSelectCity(item) {
      console.log(item);
    },
    querySearchCity(queryString, cb) {
      this.getCity(queryString, cb);
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
//选择人数
.select_num {
  position: relative;
  .num {
    width: 300px;
    transform-origin: center top;
    z-index: 10;
    position: absolute;
    top: 50px;
    left: 0px;
    display: block;
    background-color: #fff;
    display: none;
    > div {
      padding-bottom: 10px;
    }
    .btn {
      border-top: 1px solid #ccc;
      padding: 10px 0 0;
    }
  }
}
</style>