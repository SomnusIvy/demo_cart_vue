<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <!-- <input type="checkbox" class="custom-control-input" id="cb1" :checked="true" /> -->
        <input type="checkbox" class="custom-control-input" :id="'cb'+id" :checked="state" @change="stateChange"/>
        <label class="custom-control-label" :for="'cb'+id">
          <!-- 商品的缩略图 -->
          <!-- <img src="../../assets/logo.png" alt="" /> -->
          <img :src="pic" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <!-- <h6 class="goods-title">商品名称商品名称商品名称商品名称</h6> -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <!-- <span class="goods-price">￥0</span> -->
        <span class="goods-price">￥{{ price }}</span>
        <!-- 商品的数量 -->
        <Counter :num="count" :id="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from '@/components/Counter/Counter.vue'


export default {
  props:{
    id:{  // 将来子组件中商品的勾选状态变化后，需要通过子传父形式，通知父组件根据id修改对应商品的勾选状态。
      required:true,
      type:Number,
    },
    // 要渲染的商品的标题
    title:{
      default:'',
      type:String,
    },
    pic:{
      default:'',
      type:String,
    },
    price:{
      default:0,
      type:Number,
    },
    // 勾选属性
    state:{
      default:true,
      type:Boolean,
    },
    count:{
      type:Number,
      default:1
    }
  },
  methods:{
    stateChange(e){
      // console.log('ok');
      // console.log(e);

      const newState = e.target.checked
      // console.log(newState);
      // console.log(this);
      // console.log(this.id);

      this.$emit('state-change',{id:this.id,value:newState})
    }
  },
  components:{
    Counter,
  }
}
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
