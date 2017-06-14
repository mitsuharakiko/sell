<template>
	<div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border_px">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span>￥{{food.price}}</span>
                  <span v-show="food.oldPrice">￥{{food.oldPrcie}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
	</div>
</template>

<script>
const ERR_OK = 0;

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data: function () {
    return {
      goods: []
    }
  },
  created: function(){
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('/api/goods').then(function(response){
      response = response.body;
      if(response.errno === ERR_OK){
        this.goods = response.data;
      }
    });
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
  }
}
</style>
