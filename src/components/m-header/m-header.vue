<template>
  <div class="m-header" ref="header">
    <div class="fallBack" @click="back" ref="fallBack">
      <svg viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" width="30" height="30">
        <path
          d="M275.2 448l166.4-166.4c12.8-12.8 12.8-32 0-44.8s-32-12.8-44.8 0L153.6 480l249.6 249.6c12.8 12.8 32 12.8 44.8 0s12.8-32 0-44.8L275.2 512H832c19.2 0 32-12.8 32-32 0-12.8-12.8-32-32-32H275.2z"
          fill="#ffffff"></path>
      </svg>
    </div>
    <input type="text" id="searchInput" class="searchInput" v-model="inputQuery" ref="searchInput" placeholder="搜索音乐、歌词、歌单">
    <span class="icon" @click="clear" v-show="inputQuery">
      <i class="icon-delete"></i>
    </span>
    <div class="maike" ref="maike">
      <svg viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" width="22" height="22">
        <path
          d="M512 703.6928c123.4944 0 223.8464-100.4544 223.8464-223.8464v-256C735.8464 100.4544 635.4944 0 512 0S288.1536 100.4544 288.1536 223.8464v255.8976c0 123.4944 100.352 223.9488 223.8464 223.9488z m383.7952-223.9488h-64c0 176.3328-143.4624 319.7952-319.7952 319.7952-176.3328 0-319.7952-143.4624-319.7952-319.7952h-64c0 200.8064 155.136 365.8752 351.8464 382.1568v97.0752H376.9344c-1.7408 0-3.3792 0.2048-4.9152 0.512-13.9264 2.3552-24.6784 14.5408-24.6784 29.184v5.632c0 14.6432 10.752 26.8288 24.6784 29.184 1.6384 0.3072 3.2768 0.512 4.9152 0.512h270.0288c1.7408 0 3.3792-0.2048 4.9152-0.512 13.9264-2.3552 24.6784-14.5408 24.6784-29.184v-5.632c0-14.6432-10.752-26.8288-24.6784-29.184-1.6384-0.3072-3.2768-0.512-4.9152-0.512H543.9488v-97.0752c196.7104-16.2816 351.8464-181.3504 351.8464-382.1568z"
          fill="#ffffff"></path>
      </svg>
    </div>
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
    <div class="headerBar">
      <div class="search-box" ref="searchBox">
        <div @click="toSearch()" class="search">
          <i class="icon-search"></i>
          <span class="search-font">搜索</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {debounce} from 'common/js/util'
  import {mapMutations, mapGetters} from 'vuex'

  export default {
    data() {
      return {
        inputQuery: '',
        currentLeft: null
      }
    },
    mounted () {
      setTimeout(() => {
        this.getBallLeft()
      }, 20)
    },
    methods: {
      ...mapMutations([
        'SET_QUERY'
      ]),
      changeIndex (id) {
        document.getElementById(id).classList.add('tabAnimation')
        setTimeout(() => {
          document.getElementById(id).classList.remove('tabAnimation')
        }, 400)
      },
      getBallLeft () {
        let el = document.getElementById('tab')
        this.currentLeft = `${Math.floor(el.clientWidth / 3) / 2 - 47}px`
      },
      toSearch () {
        this.$router.push({path: '/search'})
        this.$refs.header.classList.add('topBar-move-up')
        this.$refs.searchBox.classList.remove('searchAppear')
        this.$refs.searchBox.classList.add('searchDisappear')
        setTimeout(() => {
          this.$refs.fallBack.style.left = '0px'
          this.$refs.maike.style.right = '0px'
          this.$refs.searchInput.style.display = 'block'
          this.$refs.searchInput.focus()
        }, 400)
      },
      back() {
        this.$refs.fallBack.style.left = '-30px'
        this.$refs.maike.style.right = '-30px'
        setTimeout(() => {
          this.$refs.header.classList.remove('topBar-move-up')
          this.$refs.searchBox.classList.remove('searchDisappear')
          this.$refs.searchBox.classList.add('searchAppear')
          this.$refs.searchInput.style.display = 'none'
          this.$router.back()
        }, 400)
      },
      clear() {
        this.inputQuery = ''
      },
      setQuery(query) {
        this.inputQuery = query
      },
      blur() {
        this.$refs.searchInput.blur()
      }
    },
    computed: {
      ...mapGetters([
        'query'
      ])
    },
    watch: {
      query() {
        this.inputQuery = this.query
      }
    },
    created() {
      this.$watch('inputQuery', debounce((newQuery) => {
        this.SET_QUERY(newQuery.trim())
      }, 200))
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .m-header
    position relative
    height 77px
    text-align center
    color $color-theme
    background $color-theme-background
    font-size 0
    .tab
      position relative
      overflow hidden
      display flex
      height 40px
      line-height 40px
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

  .tabAnimation
    position absolute
    top -27px
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
  .topBar-move-up
    animation-name tmu
    animation-duration .4s
    animation-iteration-count 1
    animation-fill-mode forwards

  @keyframes tmu {
    from {
    }
    to {
      transform: translate3d(0, -40px, 0)
    }
  }
  .headerBar
    display flex
    justify-content center
    height 35px
    line-height 35px
    .search-box
      width 100%
      margin 0 8px
      .search
        display flex
        position inherit
        justify-content center
        align-items center
        box-sizing border-box
        border-radius 6px
        width 100%
        height 30px
        background #29b16c
        transition: all .3s
        .icon-search
          font-size 24px
          color $color-theme
        .search-font
          letter-spacing 2px
          font-size $font-size-medium-x
          color $color-theme
  .fallBack
    position fixed
    width 30px
    height 30px
    left -30px
    top 44px
    transition: all .3s
  .maike
    position fixed
    display flex
    justify-content center
    align-items center
    width 30px
    height 30px
    right -30px
    top 44px
    transition: all .3s
  .searchInput
    display none
    position fixed
    top 49px
    left 30px
    height 20px
    line-height 20px
    width 80%
    color #ffffff
    background transparent
    border none
    -webkit-appearance textfield
    font-size 14px
    transition all .3s
    &::-webkit-input-placeholder
      color rgba(255,255,255,.6)
  .searchDisappear
    animation-name sd
    animation-duration .6s
    animation-iteration-count 1
    animation-fill-mode forwards

  @keyframes sd {
    from {
      opacity 1
      width 100%
    }
    to {
      opacity 0
      width 60%
    }
  }
  .searchAppear
    animation-name sa
    animation-duration .6s
    animation-iteration-count 1
    animation-fill-mode forwards

  @keyframes sa {
    from {
      opacity 0
      width 60%
    }
    to {
      opacity 1
      width 100%
    }
  }
  .icon
    extend-click()
    position fixed
    width 30px
    height 30px
    right 0px
    top 44px
    .icon-delete
      font-size: $font-size-small
      color: $color-text-d
</style>
