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
      <div v-if="seller.supports" class="support-count" @click="showDetail">
      	<span class="count">{{ seller.supports.length }}个</span>
      	<i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
    	<span class="bulletin-title"></span><span class="bulletin-text">{{ seller.bulletin }}</span>
    	<i class="icon-keyboard_arrow_right"></i>
    </div>

    <div class="background">
    	<img :src="seller.avatar" width="100%" height="100%">
    </div>
    <!-- 浮层 -->
    <div v-show="detailShow" class="detail">
    	<div class="detail-wrapper clearfix">
    		<div class="detail-main">
    			<h1 class="name">{{ seller.name }}</h1>
    			<star :size="48" :score="seller.score"></star>
    		</div>
    	</div>
    	<div class="detail-close" @click="closeDetail">
    		<i class="icon-close"></i>
    	</div>
    </div>
  </div>
</template>

<script>
import star from '../star/star';

    export default {
      props: { seller: { type: Object } },
      created: function () {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      },
      data: function () { return { detailShow: false }; },
      methods: { showDetail: function () { this.detailShow = true; }, closeDetail: function () { this.detailShow = false; } },
      components: { star }
    };
</script>

<style lang="scss" scoped>
@import "../../common/css/mixin";
@import "../../common/css/icon.css";



.header {
  color: #fff;
  position: relative;
  background: rgba(7,17,27,0.5);
  overflow: hidden;

  .content-wrapper {
    padding: 24px 12px 18px 24px;
    font-size: 0;
    position: relative;
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
          line-height: 10px;
        }
      }
    }

    .support-count {
    	position: absolute;
    	right: 12px;
    	bottom: 14px;
    	padding: 0 8px;
    	height: 24px;
    	line-height: 24px;
    	border-radius: 14px;
    	background: rgba(0,0,0,0.2);
    	text-align: center;

    	.count {
    		vertical-align: top;
    		font-size: 10px;
    	}
    	.icon-keyboard_arrow_right {
    		line-height: 24px;
    		margin-left: 2px;
    		font-size: 10px;
    	}
    }
  }

  .bulletin-wrapper {
  	padding: 0 22px 0 12px;
  	height: 28px;
  	line-height: 28px;
  	white-space: nowrap;
  	overflow: hidden;
  	text-overflow: ellipsis;
  	position: relative;
  	background: rgba(7,17,27,0.2);

  	.bulletin-title {
  		display: inline-block;
  		width: 22px;
  		height: 12px;
  		@include bg_image ("bulletin");
  		background-size: 22px 12px;
  		background-repeat: no-repeat;
  		vertical-align: top;
  		margin-top: 8px;
  	}

  	.bulletin-text {
  		margin-left: 4px;
  		margin-right: 4px;
  		font-size: 10px;
  		font-weight: 200;
  		vertical-align: top;
  	}

  	.icon-keyboard_arrow_right {
		margin-left: 2px;
		font-size: 10px;
		position:absolute;
		right: 12px;
		top: 8px;
	}
  }

  .background {
  	position: absolute;
  	top: 0;
  	left: 0;
  	width: 100%;
  	height: 100%;
  	z-index: -1;
  	filter: blur(10px);
  }

  .detail {
  	position: fixed;
  	top: 0;
  	left: 0;
  	z-index: 100;
  	width: 100%;
  	height: 100%;
  	overflow: auto;
  	background: rgba(7,17,27,0.8);

  	.detail-wrapper {
  		width: 100%;
  		min-height: 100%;

  		.detail-main {
  			margin-top: 64px;
  			padding-bottom: 64px;

  			.name {
  				line-height: 16px;
  				text-align: center;
  				font-size: 16px;
  				font-weight: 700;
  			}
  		}
  	}

  	.detail-close {
		margin: -64px auto 0 auto;
		position: relative;
		width: 32px;
		height: 32px;
		clear: both;
		font-size: 32px;
	}
  }
}

</style>
