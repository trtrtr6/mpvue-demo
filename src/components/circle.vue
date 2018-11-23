<template>
  <view class="circle_box" :style="canvasSize">
    <canvas v-if="canvasId" class="circle_bg" :canvas-id="canvasId" :style="canvasSize"></canvas> 
    <canvas v-if="canvasId" class="circle_draw" :canvas-id="canvasId+'-inner'" :style="canvasSize"></canvas>
    <view class="circle_text" :hidden="isHide">
      <slot></slot>
    </view>
  </view>
</template>
<script>
export default {
  data () {
    return {
      context: null// 用来缓存创建的进度条圆环的画布实例
    }
  },
  props: {
    canvasId: {
      type: String,
      default: 'circle'
    },
    size: {
      type: Number,
      default: 100
    },
    cirBgColor: {
      type: String,
      default: '#20183b'
    },
    cirColor: {
      type: String,
      default: '#fe6400'
    },
    lineWidth: {
      type: Number,
      default: 6
    },
    percentage: {
      type: Number,
      default: 0
    },
    isHide: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    canvasSize () {
      return `width:${this.size}px;height:${this.size}px`
    }
  },
  methods: {
    drawCircleBg: function (id) {
      // 设置圆环外面盒子大小 宽高都等于圆环直径
      const x = this.size / 2
      const w = this.lineWidth
      const ctx = wx.createCanvasContext(id)
      ctx.setLineWidth(w)
      ctx.setStrokeStyle(this.cirBgColor)
      ctx.setLineCap('round')
      // 开始一个新的路径
      ctx.beginPath()
      // 设置一个原点(x,y)，半径为r的圆的路径到当前路径 此处x=y=r
      ctx.arc(x, x, x - w, 0, 2 * Math.PI, false)
      // 对当前路径进行描边
      ctx.stroke()
      ctx.draw()
    },
    drawCircle: function (id, step) {
      let ctx = this.context
      if (!ctx) {
        ctx = wx.createCanvasContext(id + '-inner')
        this.context = ctx
      }
      const x = this.size / 2
      const w = this.lineWidth
      ctx.setLineWidth(w)
      ctx.setStrokeStyle(this.cirColor)
      ctx.setLineCap('round')
      // 开始一个新的路径
      ctx.beginPath()
      // 2* step 从0到2为一周
      ctx.arc(x, x, x - w, -Math.PI / 2, 2 * step * Math.PI - Math.PI / 2, false)
      // 对当前路径进行描边
      ctx.stroke()
      ctx.draw()
    }
  },
  mounted () {
    this.drawCircleBg(this.canvasId)
    this.drawCircle(this.canvasId, this.percentage / 100)
    this.context = wx.createCanvasContext(this.canvasId + '-inner')
  },
  watch: {
    percentage (n) {
      this.drawCircle(this.canvasId, n / 100)
    }
  }
}
</script>
<style scoped>
.circle_box,.circle_draw{ position: relative; }
.circle_bg{position: absolute;z-index:-10}
.circle_text{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
</style>



