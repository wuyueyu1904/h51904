<template>
  <div id="app">
    <router-view> </router-view>
    <footer class="footer">
      <ul>
        <router-link to="/home" tag="li">
          <span class = "iconfont icon-shouye"></span>
          <p>首页</p>
        </router-link>
        <router-link to="/kind" tag="li">
          <span class = "iconfont icon-fenlei"></span>
          <p>分类</p>
        </router-link>
        <router-link to="/cart" tag="li">
          <span class = "iconfont icon-gouwuche"></span>
          <p>购物车</p>
        </router-link>
        <router-link to="/user" tag="li">
          <span class = "iconfont icon-wode"></span>
          <p>我的</p>
        </router-link>
      </ul>
    </footer>
  </div>
</template>

<style lang="scss">
@import '@/lib/reset.scss';
html, body, #app, .box {
  @include rect(100%, 100%); // width: 100%;height: 100%;
}
#app, .box {
  @include flexbox(); // display: -webkit-box;display: -ms-flexbox;display: flex;
  @include flex-direction(column); //     -webkit-box-orient: vertical;-webkit-box-direction: normal;-ms-flex-direction: column;flex-direction: column;
  .container {
    @include flex(); // -webkit-box-flex: 1;-ms-flex: 1;flex: 1;width: .1px;
    @include rect(100%, auto); // 修改上面的width
    @include flexbox();
    @include flex-direction(column);
    .header {
      @include rect(100%, 0.44rem);
      @include flexbox();
      // @include flex-direction(column);
      @include justify-content($justify-content:space-around);
      @include align-items();
    }
    .content {
      @include flex();
      @include rect(100%, auto);
      @include overflow();
    }
  }
  .footer {
    border-top:1px solid #efefef;
    @include rect(100%, 0.5rem);
    @include background-color(#ffffff);
    ul {
      @include rect(100%, 100%);
      @include flexbox();
      li {
        @include flex();
        @include rect(auto, 100%);
        @include flexbox();
        @include flex-direction(column);
        @include justify-content();
        @include align-items();
        span {
          @include font-size(24px);
        }
        p {
          @include font-size(12px);
        }

        &.router-link-active { // li.router-link-active {}
          @include color(#f66);
        }
      }
    }
  }
}
</style>
