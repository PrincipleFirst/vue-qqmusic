<template>
  <div class="m-header">
    <div class="tab" id="tab">
      <router-link tag="div" class="tab-item" to="/singer">
        <div class="tab-animation"></div>
        <span class="tab-link">我的</span>
      </router-link>
      <router-link tag="div" class="tab-item" to="/recommend">
        <div class="tab-animation"></div>
        <span class="tab-link">音乐馆</span>
      </router-link>
      <router-link tag="div" class="tab-item" to="/rank">
        <div class="tab-animation"></div>
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
  export default {
    mounted () {
      setTimeout(() => {
        this.test()
      }, 20)
    },
    methods: {
      test () {
        let el = document.getElementById('tab')
        let diameter = Math.floor(el.clientWidth / 3)
        let tabItems = el.getElementsByClassName('tab-animation')
        let forEach = Array.prototype.forEach
        forEach.call(tabItems, function (item) {
          item.style.width = `${diameter}px`
          item.style.height = `${diameter}px`
          item.style.top = `-${diameter / 2 - 22}px`
        })
      }
    }
  }
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
      position relative
      overflow hidden
      display flex
      height 44px
      line-height 44px
      font-size $font-size-medium
      .tab-item
        flex 1
        text-align center
        .tab-link
          position relative
          color $color-theme
          z-index 11
        &.router-link-active
          .tab-link
            color $color-theme
            font-size $font-size-large
            font-weight 600
            z-index 11
    .search-box
      position relative
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

  .tab-animation
    position absolute
    border-radius 50%
    background rgb(41, 177, 108)
    transition all .4s linear
</style>
