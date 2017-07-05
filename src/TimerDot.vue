<template>
<canvas id="timer-dot" :width="size" :height="size" :style="{backgroundColor: bgColor, color: color}"></canvas>
</template>

<script>
export default {
  name: 'timer-dot',
  props: [
    'bgColor',
    'color',
    'size',
    'time'
  ],
  methods: {
    draw: function(perc) {
      let radius = parseFloat(this.size) / 2
      let el = this.$el
      let ctx = el.getContext("2d")
      ctx.fillStyle = this.color
      // ctx.moveTo(radius, radius)
      ctx.translate(radius, radius)
      // ctx.rotate(45 * Math.PI / 180)
      ctx.beginPath()
      console.log("rad", radius)
      ctx.arc(0, 0, radius, perc / 50 * Math.PI, 2 * Math.PI, false)
      // ctx.arc(0, 0, radius, 0, (perc / 100) * 2 * Math.PI, false)
      // ctx.arc(radius, radius, radius, 0, (perc / 100) * 2 * Math.PI, false)
      ctx.lineTo(0, 0)
      ctx.fill()
      ctx.setTransform(1, 0, 0, 1, 0, 0)
    }
  },
  mounted: function() {
    this.draw(0)
  }
}
</script>

<style>
.timer-dot {
  /*position: relative;*/
  display: inline-block;
}
</style>
