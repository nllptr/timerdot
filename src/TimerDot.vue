<template>
<canvas id="timer-dot" :width="size" :height="size" :style="{backgroundColor: bgColor, color: color}"></canvas>
</template>

<script>
import TWEEN from '@tweenjs/tween.js'

export default {
  name: 'timer-dot',
  props: {
    bgColor: String,
    color: String,
    size: Number,
    duration: {
      type: Number,
      required: true
    }
  },
  methods: {
    draw: function(tweeningValue) {
      const radius = parseFloat(this.size) / 2
      const el = this.$el
      const ctx = this.$el.getContext("2d")
      ctx.clearRect(0, 0, this.size, this.size)
      ctx.translate(radius, radius)
      ctx.rotate(-90 * Math.PI / 180)
      ctx.beginPath()
      ctx.arc(0, 0, radius, 2 * Math.PI, (tweeningValue / this.duration) * 2 * Math.PI, true)
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
        })
        .start()
      animationFrame = requestAnimationFrame(animate)
    }
  },
  watch: {
    duration: function(startValue, endValue) {
      this.tween(startValue, endValue)
    }
  },
  mounted: function() {
    const ctx = this.$el.getContext("2d")
    ctx.fillStyle = this.color
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
