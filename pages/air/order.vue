<template>
  <div class="container">
    <el-row type="flex" justify="space-between">
      <!-- 订单表单 -->
      <div class="main">
        <!-- 订单表单 -->
        <OrderForm :data="infoData" @setAllPrice="setAllPrice"  />
      </div>

      <!-- 侧边栏 -->
      <div class="aside">
        <OrderAside :data="infoData" :allPrice="allPrice" :num="num" />
      </div>
    </el-row>
  </div>
</template>

<script>
import OrderForm from "@/components/air/orderForm.vue";
import OrderAside from "@/components/air/orderAside.vue";

export default {
  data() {
    return {
      // 机票信息
      infoData: {
        insurances: [], // 初始化保险数据
        seat_infos: {}
      },
      allPrice: 0,
      num: 1 //乘机人数
    };
  },
  components: {
    OrderForm,
    OrderAside
  },
  mounted() {
    const { query } = this.$route;

    this.$axios({
      url: `airs/${query.id}`,
      params: {
        seat_xid: query.seat_xid
      }
    }).then(res => {
      this.infoData = res.data;
    });
  },
  methods: {
    //计算总价
    setAllPrice(price,num) {
      this.allPrice = price;
      this.num = num;
    }
  }
};
</script>

<style lang="less" scoped>
.container {
  width: 1000px;
  margin: 20px auto;
}

/*aside*/
.aside {
  width: 350px;
  height: fit-content;
  border: 1px #ddd solid;
}
</style>