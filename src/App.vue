<template>
  <div class="app-container">
    <Header title="购物车案例"></Header>
    <!-- <h1>App 根组件</h1> -->

    <!-- <p>{{ fullState }}</p> -->
    <!-- <p>{{ amt }}</p> -->

    <Goods 
    v-for="item in list" 
    :key="item.goods_id" 
    :id="item.goods_id" 
    :title="item.goods_name" 
    :pic="item.goods_img" 
    :price="item.goods_price" 
    :state="item.goods_state"
    :count="item.goods_count"
    @state-change="getNewState"
    ></Goods>

    <Footer :isfull="fullState" :amount="amt" :all="total" @full-change="getFullState"></Footer>

  </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'

import bus from '@/components/eventBus'


export default {
  data(){
    return {
      // 存储购物车的列表数据，默认为空数组
      list:[]
    }
  },
  // 购物车计算属性
  computed:{
    fullState(){
      return this.list.every(item => item.goods_state)
    },
    // 已勾选商品的总价格
    amt(){
      // 先过滤再累加
      return this.list
      .filter(item => item.goods_state)
      // 下面reduce里面，total计算公式前，需要加return，但因为只有一行，所以可以把=>后面的大括号去掉，return去掉。
      .reduce((total,item)=> total += item.goods_price* item.goods_count, 0)
    },
    // 购物车总数量
    total(){
      return this.list
      .filter(item => item.goods_state)
      .reduce((t,item)=>{return t += item.goods_count},0)
    }
  },
  created() {
    // 请求数据
    this.initCartList(),
    bus.$on('share',(val)=>{
      // console.log('接受',val);
      this.list.some(item=>{
        if(item.goods_id === val.id){
          item.goods_count = val.value
          return true
        }
      })
    })
  },
  components:{
    Header,
    Goods,
    Footer, 
  },
  methods:{
    async initCartList(){
      const {data:res} = await axios.get('https://applet-base-api-t.itheima.net/api/cart')  // :重命名
      // console.log(res);
      // 请求回来的数据，在页面渲染期间用到，必须转存到data
      if(res.status === 200){
        this.list = res.list
      }
    },
    getNewState(e){  // 接受子组件传递过来的数据
      console.log(e);
      this.list.some(item=>{
        if(item.goods_id === e.id){  // ！！！！！！！！！！！！！！！看数据的id名！！！！！！！！！
          item.goods_state = e.value
          return true
        }
      })
    },
    // 接受Footer子组件传来的全选按钮状态
    getFullState(val){
      // console.log(val);
      this.list.forEach(item => item.goods_state = val)
    }
  },

}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
