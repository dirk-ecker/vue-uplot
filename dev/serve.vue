<template>
  <div id="app">
    <vue-uplot
        no-timestamps
        :opts="opts"
        :data="data"
    />
  </div>
</template>

<script>
import Vue from 'vue'
import VueUplot from '@/vue-uplot.vue'

const boxMullerRandom = (() => {
  let phase = 0, x1, x2, w, z
  return () => {
    if (!phase) {
      do {
        x1 = 2.0 * Math.random() - 1.0
        x2 = 2.0 * Math.random() - 1.0
        w = x1 * x1 + x2 * x2
      } while (w >= 1.0)
      w = Math.sqrt((-2.0 * Math.log(w)) / w)
      z = x1 * w
    }
    else
      z = x2 * w
    phase ^= 1
    return z
  }
})()

function randomWalk(value, steps, min, max) {
  min = min == null ? -Infinity : min
  max = max == null ? Infinity : max
  let points = Array(steps), r
  for (let i = 0; i < steps; i++) {
    do {
      r = boxMullerRandom()
    } while (value + r < min || value + r > max)
    value += r
    points[i] = value
  }
  return points
}

let vals1 = randomWalk(50, 200, 0, 100)
let vals2 = randomWalk(50, 200, 0, 100)
let vals3 = randomWalk(50, 200, 0, 100)

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueUplot
  },
  data () {
    return {
      opts: {
        width: 800,
        height: 600,
        title: 'Points',
        scales: {
          x: {
            time: false,
          },
        },
        series: [
          {},
          {
            stroke: 'green',
          },
          {
            stroke: 'red',
            fill: 'orange',
            points: { space: 0 }
          },
          {
            stroke: 'blue',
            fill: 'yellow',
            paths: u => null,
            points: { space: 0 }
          },
        ],
      },
      data: [
        vals1.map((v, i) => i+1),
        vals1,
        vals2,
        vals3,
      ]
    }
  }
})
</script>
