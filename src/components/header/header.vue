<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{ seller.name }}</span>
        </div>
        <div class="description">
          {{ seller.description +' / '+ seller.deliveryTime + '分钟送达'}}
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{ seller.supports[0].description }}</span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper">

    </div>
  </div>
</template>

<script>
    export default {
      props: { seller: { type: Object } },
      created: function () {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      }
    };
</script>

<style lang="scss" scoped>
@import "../../common/css/mixin";

.header {
  color: #fff;
  background: #000;

  .content-wrapper {
    padding: 24px 12px 18px 24px;
    font-size: 0;
    .avatar {
      display: inline-block;
      vertical-align: top;

      img {
        border-radius: 2px;
      }
    }

    .content {
      margin-left: 16px;
      display: inline-block;

      .title {
        margin: 2px 0 8px 0;

        .brand {
          display: inline-block;
          vertical-align: top;
          width: 30px;
          height: 18px;
          @include bg_image("brand");
          background-size: 30px 18px;
          background-repeat: no-repeat;
        }

        .name {
          margin-left: 6px;
          font-size: 16px;
          line-height: 18px;
          font-weight: bold;
        }
      }

      .description {
        margin-bottom: 10px;
        line-height: 12px;
        font-size: 12px;
      }

      .support {
        .icon {
          display: inline-block;
          margin-right: 4px;
          width: 12px;
          height: 12px;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          vertical-align: top;

          &.decrease {
            @include bg_image('decrease_1');
          }

          &.discount {
            @include bg_image('discount_1');
          }

          &.guarantee {
            @include bg_image('guarantee_1');
          }

          &.invoice {
            @include bg_image('invoice_1');
          }

          &.special {
            @include bg_image('special_1');
          }
        }

        .text {
          margin-left: 4px;
          font-size: 10px;
          font-weight: 200;
          line-height: 12px;
        }
      }
    }
  }
}

</style>
