<template>
  <div ref="uplot"></div>
</template>

<script>
import  'uplot/dist/uPlot.min.css'
import uPlot from 'uplot/dist/uPlot.esm'

export default {
  name: 'VueUplot',
  props: {
    opts: {
      type: Object,
      required: true
    },
    data: {
      type: Array,
      required: true
    }
  },
  mounted() {
    this.uplot = new uPlot(this.opts, this.data, this.$refs.uplot)
  },
  beforeDestroy() {
    delete this.uplot
  }
}
</script>
