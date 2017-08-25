<template>
  <div class="m-header">
    <div class="tab" id="tab">
      <router-link v-on:click.native="changeIndex('singerTab')" tag="div" class="tab-item" to="/singer">
        <div id="singerTab" :style="{marginLeft:currentLeft}"></div>
        <span class="tab-link">我的</span>
      </router-link>
      <router-link v-on:click.native="changeIndex('recommendTab')" tag="div" class="tab-item" to="/recommend">
        <div id="recommendTab" :style="{marginLeft:currentLeft}"></div>
        <span class="tab-link">音乐馆</span>
      </router-link>
      <router-link v-on:click.native="changeIndex('rankTab')" tag="div" class="tab-item" to="/rank">
        <div id="rankTab" :style="{marginLeft:currentLeft}"></div>
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
    data() {
      return {
        currentLeft: null
      }
    },
    mounted () {
      setTimeout(() => {
        this.test()
      }, 20)
    },
    methods: {
      changeIndex (id) {
        document.getElementById(id).classList.add('tabAnimation')
        setTimeout(() => {
          document.getElementById(id).classList.remove('tabAnimation')
        }, 400)
      },
      test () {
        let el = document.getElementById('tab')
        this.currentLeft = `${Math.floor(el.clientWidth / 3) / 2 - 47}px`
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
        position relative
        flex 1
        text-align center
        .tab-link
          position relative
          color $color-theme
        &.router-link-active
          .tab-link
            color $color-theme
            font-size $font-size-large
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
  .tabAnimation
    position absolute
    top -25px
    height 94px
    width 94px
    border-radius 50%
    background rgb(41, 177, 108)
    animation-name ball
    animation-duration .4s
    animation-iteration-count 1

    @keyframes ball {
      from {
        transform scale(0)
      }
      to {
        transform scale(1)
      }
    }
</style>
