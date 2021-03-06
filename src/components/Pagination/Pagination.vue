<template>
  <div :class="[prefixCls + '-pagination']">
  <template v-if="totalPage > 1">
    <options :total="total" :default-size="pageSize" :page-size-options="pageSizeOptions" :show-size-changer="showSizeChanger" @pagination-size-change="changePageSize"></options>
    <jumper
        :quick-go="showJumper ? _handleChange.bind(this) : null"
        :curr-page="currPageNum"
        :total-page="totalPage"
        :mini="mini"
    ></jumper>
    <pager :page-range="pageRange" :simple="simple"  :mini="mini" :page-click="pageClick"></pager>
  </template>
  </div>
</template>

<script>
import jumper from './Jumper.vue'
import pager from './Pager.vue'
import Options from './Options.vue'

export default {
  name: 'Pagination',
  props: {
    pageSize: {
      type: Number,
      default: 10
    },
    pageSizeOptions: {
      type: Array,
      default () {
        return [10, 20, 30, 40]
      }
    },
    total: Number,
    currPage: {
      type: Number,
      default: 1
    },
    showJumper: Boolean,
    showSizeChanger: Boolean,
    simple: Boolean,
    mini: Boolean,
    prefixCls: {
      type: String,
      default: 'atui'
    }
  },
  data () {
    return {
      pageRange: [],
      prevShow: 1,
      nextShow: 1,
      totalPage: 0,
      currPageSize: this.pageSize,
      currPageNum: this.currPage
    }
  },
  created () {
    this.totalPage = Math.ceil(this.total / this.pageSize)
    this.currPageNum = this.currPage
    this.currPageSize = this.pageSize
  },
  watch: {
    total () {
      this.getPageRange()
    },
    pageSize (pageSize) {
      this.currPageSize = pageSize
    },
    currPageSize (newVal, oldVal) {
      this.totalPage = Math.ceil(this.total / newVal)
      if (this.currPageNum > this.totalPage) {
        this.currPageNum = this.totalPage
      }
      this.getPageRange()
      this.$nextTick(() => {
        this.$emit('size-change', this.currPage, newVal)
      })
    },
    currPage () {
      this.getPageRange()
      // this.onChange(this.currPageNum)
    },
    currPageNum (newVal, oldVal) {
      if (newVal !== oldVal) {
        this.$emit('change', newVal)
      }
    },
    prevShow () {
      this.getPageRange()
    },
    nextShow () {
      this.getPageRange()
    }
  },
  components: {
    jumper,
    pager,
    Options
  },
  methods: {
    changePageSize (option) {
      this.currPageSize = +option.value
      this.getPageRange()
    },
    getPageRange () {
      let start = 0
      let end = 0
      let me = this
      let showLen = me.prevShow + me.nextShow + 1
      let totalPage = me.totalPage = Math.ceil(me.total / me.currPageSize)
      let prefixCls = me.prefixCls
      let currPage = me.currPageNum

      if (totalPage <= 1) {
        start = end = 1
      } else if (me.totalPage <= showLen) {
        start = 1
        end = totalPage
      } else {
        if (currPage <= me.prevShow + 1) {
          start = 1
          end = showLen
        } else if (currPage >= totalPage - me.nextShow) {
          end = totalPage
          start = totalPage - showLen + 1
        } else {
          start = currPage - me.prevShow
          end = currPage + me.nextShow
        }
      }

      me.pageRange = []

      if (me.simple) {
        // 上一页
        if (currPage !== 1) {
          me.pageRange.push({num: currPage - 1, text: '<', className: prefixCls + '-pagination-item-prev'})
        } else {
          me.pageRange.push({className: prefixCls + '-pagination-item-disabled', icon: 'prev'})
        }

        me.pageRange.push({num: currPage, text: currPage, className: prefixCls + '-pagination-item-current'})
        me.pageRange.push({text: '/', className: prefixCls + '-pagination-item-slash'})
        me.pageRange.push({text: totalPage})

        // 下一页
        if (currPage !== totalPage) {
          me.pageRange.push({num: currPage + 1, text: '>', className: prefixCls + '-pagination-item-next'})
        } else {
          me.pageRange.push({className: prefixCls + '-pagination-item-disabled', icon: 'next'})
        }
      } else {
        // 上一页
        if (currPage !== 1) {
          me.pageRange.push({num: currPage - 1, text: '<', className: prefixCls + '-pagination-item-prev'})
        } else {
          me.pageRange.push({className: prefixCls + '-pagination-item-disabled', icon: 'prev'})
        }
        // 第一页
        if (start >= 2) {
          me.pageRange.push({num: 1, text: 1})
        }
        // 省略号
        if (start > 2) {
          me.pageRange.push({text: '...', className: prefixCls + '-pagination-item-ellipsis'})
        }
        // 显示的页码列表
        for (let i = start; i <= end; i++) {
          me.pageRange.push({
            num: i,
            text: i,
            className: (i === currPage) ? prefixCls + '-pagination-item-current' : ''
          })
        }
        // 省略号
        if (end < totalPage - 1) {
          me.pageRange.push({text: '...', className: prefixCls + '-pagination-item-ellipsis'})
        }
        // 最后一页
        if (end <= totalPage - 1) {
          me.pageRange.push({num: totalPage, text: totalPage})
        }
        // 下一页
        if (currPage !== totalPage) {
          me.pageRange.push({num: currPage + 1, text: '>', className: prefixCls + '-pagination-item-next'})
        } else {
          me.pageRange.push({className: prefixCls + '-pagination-item-disabled', icon: 'next'})
        }
      }
    },
    pageClick (i) {
      if (!i) {
        return false
      }
      if (i === this.currPageNum) {
        return false
      }
      this.currPageNum = i
      this.getPageRange()
      this.onChange(i)
    },
    onChange (pageNum) {
      // 新加的，暂时保持新老并存
      // this.$emit('change', pageNum)
    },
    _isValid (page) {
      return typeof page === 'number' && page >= 1 && page !== this.currPageNum
    },
    _handleChange (page) {
      let _page = page
      if (this._isValid(_page)) {
        if (_page > this.totalPage) {
          _page = this.totalPage
        }
        this.currPageNum = page
        this._current = page
        this.getPageRange()
        this.onChange(_page)
        return _page
      }

      return this.currPageNum
    }
  },
  mounted () {
    this.getPageRange()
  }
}
</script>
