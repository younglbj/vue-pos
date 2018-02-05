<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="posOrder">
        订单栏
      </el-col>
      <el-col :span="17">
        菜单栏
      </el-col>
    </el-row>
  </div>
</template>

<script>
import ElRow from "element-ui/packages/row/src/row";
import ElCol from "element-ui/packages/col/src/col";

export default {
  components: {
    ElCol,
    ElRow},
  name: 'Pos',
  data () {
    return {
      msg: 'Welcome to my pos system'
    }
  },
  mounted:function () {
    let currHeight = document.body.clientHeight;
    let posOrder = document.querySelector('#posOrder');
    posOrder.style.height = currHeight+'px';
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pos-order{
    background-color:#f9fafc;
    border:1px solid #c0ddca;
  }
</style>
