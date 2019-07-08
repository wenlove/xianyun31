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
      <el-form-item>
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
          <el-popover placement="bottom" width="260">
            <div class="num">
              <el-row type="flex" align="middle" :gutter="10">
                <el-col :span="4">
                  <div class="grid-content">每间</div>
                </el-col>
                <el-col :span="10">
                  <el-select size="mini" v-model="ruleForm.adult" placeholder="0 成人">
                    <el-option
                      v-for="(item,index) in adults"
                      :key="index"
                      :label="item.label"
                      :value="item.value"
                    ></el-option>
                  </el-select>
                </el-col>
                <el-col :span="10">
                  <el-select size="mini" v-model="ruleForm.children" placeholder="0 儿童">
                    <el-option
                      v-for="(item,index) in children"
                      :key="index"
                      :label="item.label"
                      :value="item.value"
                    ></el-option>
                  </el-select>
                </el-col>
              </el-row>
              <el-row class="btn" cla type="flex" align="middle" justify="end">
                <el-button type="primary" @click="handleSelectNum" size="mini">确定</el-button>
              </el-row>
            </div>
            <el-input
              placeholder="人数未定"
              suffix-icon="el-icon-user"
              v-model="ruleForm.num"
              readonly
              slot="reference"
            ></el-input>
          </el-popover>
        </el-col>
      </el-form-item>

      <!-- 确定按钮 -->
      <el-form-item>
        <el-button type="primary" @click="handleQueryCity">查看价格</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      flag:true,
      ruleForm: {
        name: "",
        city: "", //城市
        num: "", //人数
        date: [], //日期选择
        adult: "",
        children: ""
      },
      //成人下拉
      adults: [
        {
          value: "1成人",
          label: "1"
        },
        {
          value: "2成人",
          label: "2"
        },
        {
          value: "3成人",
          label: "3"
        },
        {
          value: "4成人",
          label: "4"
        },
        {
          value: "5成人",
          label: "5"
        },
        {
          value: "6成人",
          label: "6"
        }
      ],
      //儿童下拉
      children: [
        { value: 0, label: "0" },
        { value: 1, label: "1" },
        { value: 2, label: "2" },
        { value: 3, label: "3" },
        { value: 4, label: "4" }
      ],
      cityList: [], //城市列表
      city: "", //城市id
      enterTime: "", //入店时间
      leftTime: "", //离开时间
      person: 0 //人数
    };
  },
  methods: {
    getCity(queryString = "", cb) {
      this.$axios({
        url: "/cities?name=" + queryString
      }).then(res => {
        const { data } = res.data;
        // console.log(data);
        const newData = data.map(v => {
          return {
            ...v,
            value: v.name
          };
        });
        this.cityList = [...newData];
        const id = this.$route.query.city;
        const arr = this.cityList.filter(v => v.id == id);

        //判断是否输入修改目的地
        if (!queryString && !arr) {
          this.ruleForm.city = arr.join("");
        }

        // if (arr.length>0&&this.flag) {
        //   this.ruleForm.city = arr[0].name;
        //   this.$emit("setCity", this.city,this.ruleForm.city);
        // }

        cb && cb(newData);
      });
      //cd回调函数
    },
    //选择城市
    handleSelectCity(item) {
      // console.log(item);
      this.ruleForm.city=item.value;
      this.city = item.id;
      this.flag=true;
      this.$emit("setCity", this.city,this.ruleForm.city);
    },
    //获取城市下拉
    querySearchCity(queryString, cb) {
      // console.log(queryString)
      this.flag=false;
      this.getCity(queryString, cb);
    },
    //选择人数
    handleSelectNum() {
      // console.log(this.ruleForm.adult, this.ruleForm.children);
      const adult = this.ruleForm.adult;
      const children =
        this.ruleForm.children != "" ? this.ruleForm.children + " 儿童" : "";

      this.ruleForm.num = adult + children;
    },
    //查询酒店数据,提交表单数据
    handleQueryCity() {
      this.$refs["ruleForm"].validate(valid => {
        if (valid) {
          // console.log(this.ruleForm);
          this.$emit("setCity", this.city,this.ruleForm.city);
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  },
  mounted() {
    this.getCity();
    //  this.$emit("setCity", this.city,this.ruleForm.city);
  }
};
</script>

<style scoped lang="less">
//选择人数
.num {
  width: 260px;
  transform-origin: center top;
  z-index: 10;
  // position: absolute;
  top: 50px;
  left: 0px;
  display: block;
  background-color: #fff;
  > div {
    padding-bottom: 10px;
  }
  .btn {
    border-top: 1px solid #ccc;
    padding: 10px 0 0;
  }
}
</style>