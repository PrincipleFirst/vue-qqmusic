<template>
  <div class="m-header">
    <div class="tab">
      <router-link tag="div" class="tab-item" to="/singer">
        <span class="tab-link">我的</span>
      </router-link>
      <router-link tag="div" class="tab-item" to="/recommend">
        <span class="tab-link">音乐馆</span>
      </router-link>
      <router-link tag="div" class="tab-item" to="/rank">
        <span class="tab-link">发现</span>
      </router-link>
    </div>
    <div class="search-box">
      <router-link tag="div" class="search" to="/search">
        <i class="icon-search"></i>
        <span class="search-font">搜索</span>
      </router-link>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .m-header
    position relative
    height 80px
    text-align center
    color $color-theme
    background $color-theme-background
    font-size 0
    .tab
      display flex
      height 44px
      line-height 44px
      font-size $font-size-medium
      .tab-item
        flex 1
        text-align center
        .tab-link
          padding-bottom 5px
          color $color-theme
        &.router-link-active
          .tab-link
            color $color-theme
            font-size $font-size-large
            font-weight 600
    .search-box
      margin 0 8px
      .search
        display flex
        justify-content center
        align-items center
        box-sizing border-box
        border-radius 6px
        width 100%
        height 30px
        background #29b16c
        .icon-search
          font-size 24px
          color $color-theme
        .search-font
          letter-spacing 2px
          font-size $font-size-medium-x
          color $color-theme
</style>
