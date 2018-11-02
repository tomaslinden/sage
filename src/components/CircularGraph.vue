<template>
  <div class="hello">
    <div style="display: flex; justify-content: center;">
      <v-stage ref="stage" :config="configKonva">
        <!-- <v-layer ref="layer">
          <v-circle :config="configCircle"></v-circle>
        </v-layer> -->
        <v-layer :ref="value.handle" :key="value.handle" v-for="value in graphData">
          <v-arc :config="value.configArc" v-if="value.configArc"></v-arc>
          <v-text :config="value.configText" v-if="value.configText"></v-text>
        </v-layer>
      </v-stage>
    </div>
  </div>
</template>

<script>

var width = window.innerWidth;
var height = window.innerHeight;
var dimensions = [width, height];
var minDimension = Math.min(width, height)
var centerCoords = {x: dimensions[0]/2, y: dimensions[1]/2}

export default {
  name: 'CircularGraph',
  props: {
    reviewData: Object,
    graphData: Array
  },
  data: () => {
    return {
      configKonva: {
        // width: width,
        // height: height
        width: dimensions[0],
        height: dimensions[1]
      },
      configCircle: {
        x: centerCoords.x,
        y: centerCoords.y,
        radius: minDimension/3,
        // fill: "red",
        stroke: "black",
        strokeWidth: 2
      },
      configArc: {
        x: centerCoords.x,
        y: centerCoords.y,
        innerRadius: minDimension/4,
        outerRadius: minDimension/3,
        angle: 60,
        // fill: 'yellow',
        stroke: 'black',
        strokeWidth: 2
      }
    };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
