<template>
  <div class="recommend" ref="recommend">
    <scroll ref="scroll" class="recommend-content" :data="discList">
      <div>
        <div v-if="recommends.length" class="slider-wrapper">
          <div class="slider-content">
            <slider ref="slider">
              <div v-for="item in recommends">
                <a :href="item.linkUrl">
                  <img class="needsclick" @load="loadImage" :src="item.picUrl">
                </a>
              </div>
            </slider>
          </div>
        </div>
        <div class="ic-guroup">
          <div class="ic-guroup-row">
            <div class="ic-item"><i class="micon geshou"></i><span class="ic-font">歌手</span></div>
            <div class="ic-item"><i class="micon paihang"></i><span class="ic-font">排行</span></div>
            <div class="ic-item"><i class="micon diantai"></i><span class="ic-font">电台</span></div>
          </div>
          <div class="ic-guroup-row">
            <div class="ic-item"><i class="micon fenlei"></i><span class="ic-font">分类歌单</span></div>
            <div class="ic-item"><i class="micon shipin"></i><span class="ic-font">视频MV</span></div>
            <div class="ic-item"><i class="micon zhuanji"></i><span class="ic-font">数字专辑</span></div>
          </div>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">热门推荐<span class="list-button">&nbsp;&nbsp;&nbsp;&nbsp;</span></h1>
          <ul>
            <li @click="selectItem(item)" v-for="item in discList" class="item">
              <div class="icon">
                <img width="60" height="60" v-lazy="item.imgurl">
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div class="loading-container" v-show="!discList.length">
        <loading></loading>
      </div>
    </scroll>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import Slider from 'base/slider/slider'
  import Loading from 'base/loading/loading'
  import Scroll from 'base/scroll/scroll'
  import {getRecommend, getDiscList} from 'api/recommend'
  import {playlistMixin} from 'common/js/mixin'
  import {ERR_OK} from 'api/config'
  import {mapMutations} from 'vuex'

  export default {
    mixins: [playlistMixin],
    data() {
      return {
        recommends: [],
        discList: []
      }
    },
    created() {
      this._getRecommend()

      this._getDiscList()
    },
    activated() {
      setTimeout(() => {
        this.$refs.slider && this.$refs.slider.refresh()
      }, 20)
    },
    methods: {
      handlePlaylist(playlist) {
        const bottom = playlist.length > 0 ? '60px' : ''

        this.$refs.recommend.style.bottom = bottom
        this.$refs.scroll.refresh()
      },
      loadImage() {
        if (!this.checkloaded) {
          this.checkloaded = true
          setTimeout(() => {
            this.$refs.scroll.refresh()
          }, 20)
        }
      },
      selectItem(item) {
        this.$router.push({
          path: `/recommend/${item.dissid}`
        })
        this.setDisc(item)
      },
      _getRecommend() {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
          }
        })
      },
      _getDiscList() {
        getDiscList().then((res) => {
          if (res.code === ERR_OK) {
            this.discList = res.data.list
          }
        })
      },
      ...mapMutations({
        setDisc: 'SET_DISC'
      })
    },
    components: {
      Slider,
      Loading,
      Scroll
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .recommend
    position fixed
    width 100%
    top 88px
    bottom 0
    .recommend-content
      height 100%
      overflow hidden
      .slider-wrapper
        position relative
        width 100%
        height 0
        padding-top 40%
        overflow hidden
        .slider-content
          position absolute
          top 0
          left 0
          width 100%
          height 100%
      .recommend-list
        .list-title
          height 40px
          line-height 40px
          text-align center
          font-size 22px
          color $color-text
          .list-button
            display inline-block
            position absolute
            right 17px
            height 40px
            width 40px
            background-image url(lefta.png)
            background-position center
            background-size 26px 26px
            background-repeat no-repeat
        .item
          display flex
          box-sizing border-box
          align-items center
          padding 0 20px 20px 20px
          .icon
            flex 0 0 60px
            width 60px
            padding-right 20px
          .text
            display flex
            flex-direction column
            justify-content center
            flex 1
            line-height 20px
            overflow hidden
            font-size $font-size-medium
            .name
              margin-bottom 10px
              color $color-text
            .desc
              color $color-text
      .loading-container
        position absolute
        width 100%
        top 50%
        transform translateY(-50%)
      .ic-guroup
        display table
        width 100%
        height 136px
        padding-left 6px
        .ic-guroup-row
          display table-row
          font-size $font-size-medium
          height 68px
          .ic-item
            display table-cell
            padding-left 15px
            vertical-align middle
            .micon
              display inline-block
              vertical-align middle
              width 28px
              height 28px
              &.geshou
                iconImg('geshou.png')
              &.paihang
                iconImg('paihang.png')
              &.diantai
                iconImg('diantai.png')
              &.fenlei
                iconImg('fenlei.png')
              &.shipin
                iconImg('shipin.png')
              &.zhuanji
                iconImg('zhuanji.png')
            .ic-font
              margin-left 7px
              font-size $font-size-medium-x
              color $color-text
</style>
