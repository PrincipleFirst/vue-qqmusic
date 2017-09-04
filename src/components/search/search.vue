<template>
  <div class="search">
    <div ref="shortcutWrapper" class="shortcut-wrapper" v-show="!query">
      <scroll :refreshDelay="refreshDelay" ref="shortcut" class="shortcut" :data="shortcut">
        <div>
          <div class="hot-key">
            <h1 class="title">热门搜索</h1>
            <ul>
              <li @click="addQuery(item.k)" class="item" v-for="item in hotKey">
                <span>{{item.k}}</span>
              </li>
            </ul>
          </div>
          <div class="search-history" v-show="searchHistory.length">
            <h1 class="title">
              <span class="text">搜索历史</span>
              <span @click="showConfirm" class="clear">
                <i class="icon-clear"></i>
              </span>
            </h1>
            <search-list @delete="deleteSearchHistory" @select="addQuery" :searches="searchHistory"></search-list>
          </div>
        </div>
      </scroll>
    </div>
    <div class="search-result" v-show="query" ref="searchResult">
      <suggest @listScroll="blurInput" @select="saveSearch" ref="suggest" :query="query"></suggest>
    </div>
    <confirm ref="confirm" @confirm="clearSearchHistory" text="是否清空所有搜索历史" confirmBtnText="清空"></confirm>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import SearchList from 'base/search-list/search-list'
  import Scroll from 'base/scroll/scroll'
  import Confirm from 'base/confirm/confirm'
  import Suggest from 'components/suggest/suggest'
  import { getHotKey } from 'api/search'
  import { ERR_OK } from 'api/config'
  import { playlistMixin } from 'common/js/mixin'
  import { mapActions, mapGetters } from 'vuex'

  export default {
    mixins: [playlistMixin],
    data () {
      return {
        hotKey: [],
        refreshDelay: 120
      }
    },
    computed: {
      shortcut () {
        return this.hotKey.concat(this.searchHistory)
      },
      ...mapGetters([
        'searchHistory',
        'query'
      ])
    },
    created () {
      this._getHotKey()
    },
    methods: {
      handlePlaylist (playlist) {
        const bottom = playlist.length > 0 ? '60px' : ''

        this.$refs.searchResult.style.bottom = bottom
        this.$refs.suggest.refresh()

        this.$refs.shortcutWrapper.style.bottom = bottom
        this.$refs.shortcut.refresh()
      },
      showConfirm () {
        this.$refs.confirm.show()
      },
      _getHotKey () {
        getHotKey().then((res) => {
          if (res.code === ERR_OK) {
            this.hotKey = res.data.hotkey.slice(0, 10)
          }
        })
      },
      blurInput() {
//        this.$refs.searchBox.blur()
      },
      addQuery(query) {
        this.setQuery(query)
      },
      saveSearch() {
        this.saveSearchHistory(this.query)
      },
      ...mapActions([
        'setQuery',
        'clearSearchHistory',
        'saveSearchHistory',
        'deleteSearchHistory'
      ])
    },
    watch: {
      query (newQuery) {
        if (!newQuery) {
          setTimeout(() => {
            this.$refs.shortcut.refresh()
          }, 20)
        }
      }
    },
    components: {
      SearchList,
      Scroll,
      Confirm,
      Suggest
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .search
    position fixed
    top 37px
    bottom 0
    z-index 100
    width 100%
    background $color-background
    .shortcut-wrapper
      position fixed
      top 35px
      bottom 0
      width 100%
      .shortcut
        height 100%
        overflow hidden
        .hot-key
          padding 15px 15px 10px 15px
          .title
            margin-bottom 8px
            font-weight bold
            font-size $font-size-medium
            color $color-text-l
          .item
            display inline-block
            font-size 14px
            padding 0 10px
            height 30px
            line-height 30px
            color #000
            border 1px solid rgba(0, 0, 0, .6)
            border-radius 99px
            word-break keep-all
            margin-bottom 10px
            margin-right 14px
        .search-history
          position relative
          margin 0 20px
          .title
            display flex
            align-items center
            height 40px
            font-size $font-size-medium
            color $color-text-l
            .text
              flex 1
            .clear
              extend-click()
              .icon-clear
                font-size $font-size-medium
                color $color-text-d
    .search-result
      position fixed
      width 100%
      top 50px
      bottom 0
</style>
