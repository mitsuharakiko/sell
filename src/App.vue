<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import Header from './components/header/header';

  const ERR_OK = 0;

  export default {
    data: function () {
      return {
        seller: {}
      };
    },
    created: function () {
      this.$http.get('/api/seller').then(function (response) {
        response = response.body;
        if (ERR_OK === response.errno) {
          this.seller = response.data;
        }
      });
    },
    components: {
      'v-header': Header
    }
  };
</script>

<style rel="text/stylesheet" lang="scss" scoped>
@import "./common/css/mixin.scss";
.tab {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  // border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  @include border_px(rgba(7, 17, 27, 0.1));
  .tab-item {
    flex: 1;
    text-align: center;
    a {
      display: block;
      font: 200 14px/40px 'Microsoft YaHei';
      color: rgb(77, 85, 93);

      &.active {
        color: rgb(240, 20, 20);
      }
    }
  }
}
</style>
