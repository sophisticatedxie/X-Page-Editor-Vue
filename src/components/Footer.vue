/**
* Created by xiejr on 2020/07/20
*
*/

<style scoped lang="less" rel="stylesheet/less">
  .xpe_footer {
    position: absolute;
    bottom: 0;
    right: 0;
    left: 0;
    width: 100%;
    height: 49px;
    line-height: 49px;
    margin-bottom: -49px;
    border-top: 1px solid #dddddd;
    display: inline-block;
    z-index: 2500;
    background: #ffffff;
    box-shadow: 0 -2px 2px 0 rgba(0, 0, 0, 0.1);
    transition: all .5s ease-in .1s;

    &.block_expand {
      margin-bottom: 0;
    }

    &:hover {
      .handler {
        margin-top: -10px;
      }
    }

    .block_header {
      width: 100%;
      height: 30px;
      line-height: 30px;
      font-size: 24px;
      font-weight: bold;
      display: inline-block;
    }
    .block_body {
      position: absolute;
      top: 0;
      right: 0;
      left: 0;
      bottom: 0;
      z-index: 2500;
      background: #ffffff;
      text-align: center;

      .title {
        height: 30px;
        line-height: 30px;
        font-size: 24px;
        font-weight: bold;
        text-shadow: 2px 0px 10px rgba(0, 0, 0, 0.3);
        display: inline-block;
        margin: 0 auto;
      }
    }
  }
</style>

<template>
  <div :class="{'xpe_footer': true, 'block_expand': isExpand}">
    <div class="block_body">
      <XUIButton type="primary">SAVE</XUIButton>
    </div>
    <XPEHandler class="handler" mode="horizontal" position="top" :expand="isExpand" :callback="toggleHandler"></XPEHandler>
  </div>
</template>

<script>
import XPEHandler from '@/global/components/Handler.vue'

import defConfig from '@/config'
import utils from '@/global/utils'

export default {
  name: 'XPEFooter',
  components: {
    XPEHandler
  },
  props: {
    config: {
      type: Object,
      default: function () {
        return defConfig.UI.footer
      }
    }
  },
  data () {
    return {
      // 是否展开
      isExpand: true
    }
  },
  methods: {
    // 切换handler显示/隐藏
    toggleHandler: function (val) {
      let _t = this
      _t.isExpand = val !== undefined ? val : !_t.isExpand
      _t.$nextTick(function () {
        let elHeight = _t.isExpand ? _t.$el.offsetHeight : 0
        // 广播事件
        utils.bus.$emit('XPE/expand/toggle/single', {
          position: 'bottom',
          val: {
            bottom: elHeight
          }
        })
      })
    }
  },
  created: function () {
    let _t = this
    // 监听事件
    utils.bus.$on('XPE/expand/toggle/all', function (val) {
      _t.toggleHandler(val)
    })
  }
}
</script>
