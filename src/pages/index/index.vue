<template>
  <view>
    <circle :sizeRpx="200" canvasId="circle" :percentage="count">
      <span style="font-size:30rpx;">中间层</span>
    </circle>
    <circle :sizeRpx="400" canvasId="circle2" :percentage="count">
      <span>中间层2</span>
    </circle>
  </view>
</template>

<script>
import card from '@/components/card'
import circle from '@/components/circle'
export default {
  data () {
    return {
      count: 0, // 计数器，初始值为0
      countTimer: null,
      percent: 0.1
    }
  },
  components: {
    card,
    circle
  },
  methods: {
    countInterval: function () {
      this.countTimer = setInterval(() => {
        if (this.count < 100) {
        // 绘制彩色圆环进度条
          this.count++
        } else {
          clearInterval(this.countTimer)
        }
      }, 50)
    }
  },
  mounted () {
    this.countInterval()
  }
}
</script>
<style scoped>
</style>
