<template>
    <div class="plot">
        Интерполяция Лагранжа:<br>
        <div ref="plotRefLagrange"></div>
    </div>
    <div class="plot">
        Интерполяция Ньютона:<br>
        <div ref="plotRefNewton"></div>
    </div>
</template>

<script>
import functionPlot from "function-plot";

export default {
  props: {
    fnLagrangeProp: {
      type: String,
    },
    fnNewtonProp: {
        type: String,
    },
    pointsProp: {
        type: Array,
    },
    leftProp: {
      type: Number,
    },
    rightProp: {
      type: Number,
    },
    bottomProp: {
        type: Number,
    },
    topProp: {
        type: Number,
    }

  },
  watch: {
    fnLagrangeProp: function () {
       this.updatePlot(this.fnLagrangeProp, this.fnNewtonProp, this.leftProp, this.rightProp, this.bottomProp, this.topProp, this.pointsProp)
    },
  },
  methods: {
    updatePlot(fnLagrange, fnNewton, left, right, bottom, top, pointsProp) {
      functionPlot({
        target: this.$refs.plotRefLagrange,
        width: 350,
        height: 350,
        grid: true,
        disableZoom: true,
        xAxis: { domain: [left, right] },
        yAxis: { domain: [bottom, top] },
        data: [
          {
            fn: fnLagrange,
            range: [left, right],
            graphType: 'polyline',
            // nSamples: 10000
          },
          {
            points: pointsProp,
            fnType: 'points',
            graphType: 'scatter'
          }
        ]

      });
      functionPlot({
        target: this.$refs.plotRefNewton,
        width: 350,
        height: 350,
        grid: true,
        disableZoom: true,
        xAxis: { domain: [left, right] },
        yAxis: { domain: [bottom, top] },
        data: [
            {
                fn: fnNewton,
                range: [left, right],
                graphType: 'polyline',
                // nSamples: 10000
            },
            {
                points: pointsProp,
                fnType: 'points',
                graphType: 'scatter'
            }
        ]
      });
    }
  }
}

</script>

<style scoped>
.plots {
    width: 400px;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
}
.plot {
    width: 350px;
}

</style>