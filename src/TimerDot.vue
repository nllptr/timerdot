<template>
<canvas id="timer-dot" :width="size" :height="size" :style="{backgroundColor: 'rgba(0, 0, 0, 0)'}"></canvas>
</template>

<script>
import TWEEN from '@tweenjs/tween.js'

export default {
  name: 'timer-dot',
  props: {
    bgColor: {
      type: String,
      default: '#ccc'
    },
    color: {
      type: String,
      default: '#fff'
    },
    borderColor: {
      type: String,
      default: '#000'
    },
    borderWidth: {
      type: Number,
      default: 0
    },
    size: {
      type: Number,
      default: 25
    },
    duration: {
      type: Number,
      required: true
    }
  },
  methods: {
    draw: function(tweeningValue) {
      const radius = this.size / 2
      const el = this.$el
      const ctx = this.$el.getContext("2d")
      ctx.clearRect(0, 0, this.size, this.size)
      ctx.translate(radius, radius)
      ctx.rotate(-90 * Math.PI / 180)

      // Background
      ctx.fillStyle = this.bgColor
      ctx.beginPath()
      ctx.arc(0, 0, radius, 0, 2 * Math.PI, true)
      ctx.fill()

      // Border
      if (this.borderWidth > 0) {
        ctx.lineWidth = this.borderWidth
        ctx.strokeStyle = this.borderColor
        ctx.beginPath()
        ctx.arc(0, 0, radius - (this.borderWidth / 2), 0, 2 * Math.PI, true)
        ctx.stroke()
      }

      // Animated part
      ctx.fillStyle = this.color
      ctx.beginPath()
      ctx.arc(0, 0, radius - this.borderWidth, 2 * Math.PI, (tweeningValue / this.duration) * 2 * Math.PI, true)
      ctx.lineTo(0, 0)
      ctx.fill()
      ctx.setTransform(1, 0, 0, 1, 0, 0)
    },
    tween: function(startValue, endValue) {
      var vm = this
      var animationFrame

      function animate(time) {
        TWEEN.update(time)
        requestAnimationFrame(animate)
      }
      new TWEEN.Tween({
          tweeningValue: startValue
        })
        .to({
          tweeningValue: endValue
        }, this.duration)
        .onUpdate(function() {
          vm.draw(this.tweeningValue)
        })
        .onComplete(function() {
          cancelAnimationFrame(animationFrame)
          vm.$emit('timerCompleted', vm)
        })
        .start()
      animationFrame = requestAnimationFrame(animate)
    },
    restart: function() {
      this.tween(0, this.duration)
    }
  },
  watch: {
    duration: function(startValue, endValue) {
      this.tween(startValue, endValue)
    }
  },
  mounted: function() {
    this.tween(0, this.duration)
  }
}
</script>

<style>
.timer-dot {
  /*position: relative;*/
  display: inline-block;
}
</style>
