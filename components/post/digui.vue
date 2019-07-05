<template>
  <div>
    <div class="item" v-for="(item,index) in data" :key="index">
        {{item.type}}
      <digui :data="item.children" v-if="item.children"></digui>
    </div>
  </div>
</template>

<script>
export default {
  name: "digui",
  props: {
    data: {
      type: Array,
      default: []
    }
  }
};
</script>

<style>
.item >div {
    margin-left: 20px;
}
</style>
