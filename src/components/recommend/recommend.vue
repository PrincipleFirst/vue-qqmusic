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
            <div class="ic-item"><div class="ic-item-container"><i class="micon geshou"></i><span class="ic-font">歌手</span></div></div>
            <div class="ic-item"><div class="ic-item-container"><i class="micon paihang"></i><span class="ic-font">排行</span></div></div>
            <div class="ic-item"><div class="ic-item-container"><i class="micon diantai"></i><span class="ic-font">电台</span></div></div>
          </div>
          <div class="ic-guroup-row">
            <div class="ic-item"><div class="ic-item-container"><i class="micon fenlei"></i><span class="ic-font">分类歌单</span></div></div>
            <div class="ic-item"><div class="ic-item-container"><i class="micon shipin"></i><span class="ic-font">视频MV</span></div></div>
            <div class="ic-item"><div class="ic-item-container"><i class="micon zhuanji"></i><span class="ic-font">数字专辑</span></div></div>
          </div>
        </div>
        <div class="recommend-list">
          <h2 class="list-title"><span class="list-title-text">热门推荐</span><a class="list-button">&nbsp;&nbsp;&nbsp;&nbsp;</a>
          </h2>
          <ul class="album_list">
            <li @click="selectItem(item)" v-for="(item, index) in discList" v-if="index<=5" class="item">
              <div class="album_list_box">
                <div class="icon">
                  <img class="album_img" v-lazy="item.imgurl">
                  <h3 class="desc" v-html="item.dissname"></h3>
                </div>
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
  import { getRecommend, getDiscList } from 'api/recommend'
  import { playlistMixin } from 'common/js/mixin'
  import { ERR_OK } from 'api/config'
  import { mapMutations } from 'vuex'

  export default {
    mixins: [playlistMixin],
    data () {
      return {
        recommends: [],
        discList: []
      }
    },
    created () {
      this._getRecommend()

      this._getDiscList()
    },
    activated () {
      setTimeout(() => {
        this.$refs.slider && this.$refs.slider.refresh()
      }, 20)
    },
    methods: {
      handlePlaylist (playlist) {
        const bottom = playlist.length > 0 ? '60px' : ''

        this.$refs.recommend.style.bottom = bottom
        this.$refs.scroll.refresh()
      },
      loadImage () {
        if (!this.checkloaded) {
          this.checkloaded = true
          setTimeout(() => {
            this.$refs.scroll.refresh()
          }, 20)
        }
      },
      selectItem (item) {
        this.$router.push({
          path: `/recommend/${item.dissid}`
        })
        this.setDisc(item)
      },
      _getRecommend () {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
          }
        })
      },
      _getDiscList () {
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
    top 80px
    bottom 0
    .recommend-content
      height 100%
      overflow hidden
      .slider-wrapper
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
          position relative
          padding 0 40px
          height 55px
          line-height 55px
          text-align center
          color $color-text
          .list-title-text
            margin-left 5px
            overflow hidden
            line-height 1.2
            white-space nowrap
            letter-spacing 5px
            font-size 18px
          .list-button
            position absolute
            top 0
            right 0
            height 100%
            width 50px
            background-image url(lefta.png)
            background-position center
            background-size 20px 20px
            background-repeat no-repeat
        .album_list
          overflow hidden
          margin 0 -1px
          .album_list_box
            margin 0 1px
          .item
            position relative
            float left
            width 33.3%
            margin-bottom 10px
            .icon
              position relative
              overflow hidden
              display block
              padding-top 100%
              .album_img
                position absolute
                top 0
                left 0
                width 100%
              .desc
                -webkit-box-orient vertical
                -webkit-line-clamp 2
                height 36px
                color $color-text
                font-size $font-size-small
                font-weight 400
                margin 5px 8px 0 0
                overflow hidden
                white-space normal
                text-overflow ellipsis
                line-height 16px
      .loading-container
        position absolute
        width 100%
        top 50%
        transform translateY(-50%)
      .ic-guroup
        display table
        width 100%
        height 136px
        text-align center
        .ic-guroup-row
          display table-row
          font-size $font-size-medium
          height 68px
          .ic-item
            display table-cell
            vertical-align middle
            .ic-item-container
              display inline-block
              width 91px
              text-align left
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
                color $color-text
</style>
