<template>
	<div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}" @click="selectMenu(index,$event)">
          <span class="text border_px">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border_px">
              <div class="icon">
                <img width="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrcie}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
	</div>
</template>

<script>
import BScroll from 'better-scroll';
import shopcart from '../shopcart/shopcart';
import cartcontrol from '../cartcontrol/cartcontrol';

const ERR_OK = 0;

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data: function () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    }
  },
  computed: {
    currentIndex: function (){
      for(let i = 0; i < this.listHeight.length ; i++){
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
          return i;
        }
      }
      return 0;
    },
    selectFoods: function () {
      var foods = [];
      this.goods.forEach(function(good){
        good.foods.forEach(function(food) {
          if(food.count){
            foods.push(food);
          }
        });
      });
      return foods;
    }
  },
  created: function(){
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('/api/goods').then(function(response){
      response = response.body;
      if(response.errno === ERR_OK){
        this.goods = response.data;
        this.$nextTick(function(){
          this._initScroll();
          this._caculateHeight();
        });
      }
    });
  },
  methods: {
    selectMenu: function (index,$event) {
      //消除浏览器本身的点击事件
      if(!$event._constructed){
        return;
      }
      let foodList = this.$refs.foodWrapper.getElementsByClassName("food-list-hook");
      let el = foodList[index];
      this.foodScroll.scrollToElement(el, 500);
    },
    _initScroll: function (){
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      });
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {
        click: true,
        probeType: 3
      });

      this.foodScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _caculateHeight: function(){
      let foodList = this.$refs.foodWrapper.getElementsByClassName("food-list-hook");
      let height = 0;
      this.listHeight.push(height);
      for(let i = 0 ; i < foodList.length; i ++){
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }
  },
  components: {
    shopcart,
    cartcontrol
  }
};
</script>

<style lang="scss" scoped>
@import "../../common/css/mixin";

.goods {
  display: flex;
  position: absolute;
  width: 100%;
  top: 174px;
  bottom: 46px;
  overflow: hidden;

  .menu-wrapper {
    flex: 0 0 80px;
    width: 80px;
    background-color: #f3f5f7;
    .menu-item {
      display: table;
      padding: 0 12px;
      height: 54px;
      width: 56px;
      line-height: 14px;
      &.current {
        position: relative;
        margin-top: -1px;
        z-index: 10;
        font-weight: 700;
        background: #fff;

        .text {
          border: none;
        }
      }

      .icon {
        display: inline-block;
        margin-right: 2px;
        width: 12px;
        height: 12px;
        background-size: 12px 12px;
        background-repeat: no-repeat;
        vertical-align: top;

        &.decrease {
          @include bg_image('decrease_3');
        }

        &.discount {
          @include bg_image('discount_3');
        }

        &.guarantee {
          @include bg_image('guarantee_3');
        }

        &.invoice {
          @include bg_image('invoice_3');
        }

        &.special {
          @include bg_image('special_3');
        }
      }

      .text {
        display: table-cell;
        width: 56px;
        vertical-align: middle;
        @include border_px(rgba(7,17,27,0.1));
        font-size: 12px;
      }
    }
  }

  .foods-wrapper {
    flex: 1;
    
    .title {
      padding-left: 14px;
      height: 26px;
      line-height: 26px;
      border-left: 2px solid #d9dde1;
      font-size: 12px;
      font-weight: 400;
      color: rgb(147,153,159);
      background: #f3f5f7;
    }

    .food-item {
      display: flex;
      margin: 18px;
      padding-bottom: 18px;
      @include border_px(rgba(7,17,27,0.1));
      &:last-child {
        @include border_none;
        margin-bottom: 0;
      }

      .icon {
        flex: 0 0 57px;
        margin-right: 10px;
      }

      .content {
        flex: 1;

        .name {
          margin: 2px 0 8px 0;
          height: 14px;
          line-height: 14px;
          font-size: 14px;
          color: rgb(7,17,27);
        }

        .desc {
          margin-bottom: 8px;
          line-height: 14px;
          font-size: 10px;
          color: rgb(147,153,159);
        }

        .extra {
          line-height: 10px;
          font-size: 10px;
          color: rgb(147,153,159);

          .count {
            margin-right: 12px;
          }
        }

        .price {
          font-weight: 700;
          line-height: 24px;
          .now {
            margin-right: 8px;
            font-size: 14px;
            color: rgb(240,20,20);
          }

          .old {
            text-decoration: line-through;
            font-size: 10px;
            color: rgb(147,153,159);
          }
        }

        .cartcontrol-wrapper {
          position: absolute;
          right: 0;
          bottom: 12px;
        }
      }
    }
  }
}
</style>
