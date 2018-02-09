<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="posOrder">
        <el-tabs type="card">
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%;" >
              <el-table-column header-align=center prop="goodsName" label="商品"></el-table-column>
              <el-table-column header-align=center prop="count" label="数量" width="70"></el-table-column>
              <el-table-column header-align=center prop="price" label="金额" width="80"></el-table-column>
              <el-table-column header-align=center label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="deleteGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <span>
                <small>数量：</small> {{totalCount}}
              </span>
              <span>
                <small>合计：</small>{{totalMoney}}元
              </span>
            </div>
            <div style="margin-top:20px;">
              <el-button type="warning" >挂单</el-button>
              <el-button type="danger" @click="delterAllGoods()">删除</el-button>
              <el-button type="success" >结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
            外卖
          </el-tab-pane>
        </el-tabs>
      </el-col>

      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs class="goods-tab">
            <el-tab-pane label="汉堡">
              <ul class='cookList'>
                <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg"  width="100%"></span>
                  <div class="goods-info">
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class='cookList'>
                <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg"  width="100%"></span>
                  <div class="goods-info">
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class='cookList'>
                <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg"  width="100%"></span>
                  <div class="goods-info">
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class='cookList'>
                <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg"  width="100%"></span>
                  <div class="goods-info">
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import ElRow from "element-ui/packages/row/src/row";
import ElCol from "element-ui/packages/col/src/col";
import ElTabPane from "../../../node_modules/element-ui/packages/tabs/src/tab-pane.vue";
import axios from "axios"
export default {
  components: {
    ElTabPane,
    ElCol,
    ElRow
  },
  name: 'Pos',
  data () {
    return {
      msg: 'Welcome to my pos system',
      totalCount:0,
      totalMoney:0,

      tableData: [],  //选择的商品
      oftenGoods:[],  //常用商品
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[]
    }
  },
  created(){
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
        this.oftenGoods = response.data;
      })
      .catch(error=>{
        console.log(error);
        alert('网络错误，不能访问');
      });
    axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      }).catch(error=>{
      console.log(error);
      alert('网络错误，不能访问');
    });
  },
  mounted:function () {  //dom加载完毕执行
    let currHeight = document.body.clientHeight;
    let posOrder = document.querySelector('#posOrder');
    posOrder.style.height = currHeight+'px';
  },
  methods:{
    addOrderList(goods){

      let isHave = false;
      for(let i=0;i<this.tableData.length;i++){
        if(this.tableData[i].goodsId==goods.goodsId){
          isHave = true;
        }
      }
      if(isHave){
        let arr = this.tableData.filter(item=>item.goodsId == goods.goodsId);
        arr[0].count++;
      }else{
        let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
//        var count = {"count":1};
//        var newGoods = Object.assign(goods,count);
        this.tableData.push(newGoods);
      }
      this.getTotalInfo();
    },
    deleteGoods(goods){
//      console.log("goods"+goods);
//      console.log("111111--------"+goods.count);
      let currentCount = goods.count;
      if(currentCount > 1){
        console.log("111");
        let arr = this.tableData.filter(item=>item.goodsId==goods.goodsId);
        arr[0].count--;
      }else{
        console.log("222");
        this.tableData = this.tableData.filter(item=>item.goodsId!==goods.goodsId);
      }
      this.getTotalInfo();
    },
    delterAllGoods(){
      this.totalCount = 0;
      this.totalMoney = 0;
      this.tableData = [];
    },
    getTotalInfo(){
      //总价统计
      this.totalCount = 0;
      this.totalMoney = 0;
      this.tableData.forEach((item)=>{
        this.totalCount+=item.count;
        this.totalMoney+=(item.price*item.count);
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pos-order{
    background-color:#f9fafc;
    border:1px solid #c0ddca;
  }
  .title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
    text-align: left;
  }
  .often-goods-list ul li{
    list-style: none;
    float:left;
    border:1px solid #E5E9F2;
    padding:10px;
    margin:5px;
    background-color:#fff;
    cursor:pointer;
  }
  .o-price{
    color:#58B7FF;
  }
  .goods-type{
    /*padding-left:2px;*/
    clear: both;
  }
  .goods-tab{
    padding-left:10px;
  }
  .cookList li {
    width:23%;
    border:1px solid #E5E9F2;
    border-radius: 5px;
    float:left;
    margin:2px 2px 2px 0;
    padding:2px 2px 2px 0;
    overflow:hidden;
    background-color:#fff;
    cursor:pointer;

  }
  /*.cookList li span{*/
    /*display: block;*/
    /*float:left;*/
  /*}*/
  .goods-info{
    float:left;
  }
  .goods-info span{
    display:block;
  }
  .foodImg{
    width:40%;
    float:left;
    display: block;
  }
  .foodName{
    font-size:18px;
    color:brown;
    padding-left:10px;
    display:block;
  }
  .foodPrice{
    font-size: 16px;
    padding-left: 10px;
    padding-top:10px;
    display:block;
    text-align: left;
  }
  .total{
    padding:15px;
  }
  .total span{
    padding:10px;
  }
</style>
