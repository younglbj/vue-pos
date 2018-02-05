<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-goumai"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>
</template>
<script>
  export default {
    name:'leftNav'
  }
</script>
<style>
  .left-nav{
    color:#fff;
    font-size: 10px;
    width:68px;
    background: #1D8ce0;
    height: 100%;
    float:left;
  }
  .iconfont{font-size: 24px}
  .left-nav li{
    text-align: center;
    list-style: none;
    padding:10px;
    border-bottom: 1px solid #20a0ff;
  }
</style>
