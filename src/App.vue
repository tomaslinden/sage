<template>
  <div id="app">
    <CircularGraph :reviewData="reviewData" :graphData="graphData" />
  </div>
</template>

<script>
import CircularGraph from './components/CircularGraph.vue'
import { polarToCartesian, describeArc } from './helpers.js'
// https://stackoverflow.com/a/18473154

var width = window.innerWidth;
var height = window.innerHeight;
var dimensions = [width, height];
console.log('dimensions', dimensions);
var minDimension = Math.min(width, height)
var centerCoords = {x: dimensions[0]/2, y: dimensions[1]/2}
console.log(minDimension)

var graphData = [];

let reviewData = { 
  topic: 'Topic',
  framework: {
    facets: [
      // { handle: 'big' },
      // { handle: 'solvable' },
      // { handle: 'neglected' },

      // { handle: 'goal' },
      // { handle: 'organization' },
      // { handle: 'locomotion' },
      // { handle: 'guidance' },
      // { handle: 'materiel' },
      // { handle: 'know-how' },
      // { handle: 'input/output' },

      // { handle: 'goal1' },
      // { handle: 'goal2' },
      // { handle: 'goal3' },
      // { handle: 'goal4' },
      // { handle: 'goal5' },
      // { handle: 'goal6' },
      // { handle: 'goal7' }

      { handle: 'goal' },
      { handle: 'organization' },
      { handle: 'locomotion' },
      { handle: 'guidance' },
      { handle: 'materiel' },
      { handle: 'know-how' },
      { handle: 'input/output' },
    ]
  }
}

// var angle = 0;
// var angleIncrement = 360/reviewData.framework.facets.length;
// var angleOffset = (270 - angleIncrement/2)%360;
// for (let value of reviewData.framework.facets) {
//   var configArc = {
//     x: centerCoords.x,
//     y: centerCoords.y,
//     innerRadius: minDimension/4,
//     outerRadius: minDimension/3,
//     angle: angleIncrement,
//     stroke: 'black',
//     strokeWidth: 2,
//     rotationDeg: angleOffset + angle
//   };
//   graphData.push({
//     handle: value.handle,
//     configArc
//   });
//   angle += angleIncrement;
// }

var angle = 0;
var facetAngleSize = 360/reviewData.framework.facets.length;
var initialAngleOffset = (270 - facetAngleSize/2)%360;
for (let value of reviewData.framework.facets) {
  var configArc = {
    x: centerCoords.x,
    y: centerCoords.y,
    innerRadius: minDimension/4,
    outerRadius: minDimension/3,
    angle: facetAngleSize,
    stroke: 'black',
    strokeWidth: 2,
    rotation: initialAngleOffset + angle
  };
  graphData.push({
    handle: 'facetArc_' + value.handle,
    configArc,
    type: 'facetArc'
  });
  var configText = {
    x: centerCoords.x,
    y: centerCoords.y,
    // text: 'Simple Text',
    // fontSize: 30,
    // fontFamily: 'Calibri',
    // fill: 'green'
    // x: 0,
    // y: 50,
    fill: '#333',
    fontSize: 16*minDimension/500,
    fontFamily: 'Monospace',
    text: value.handle,
    // data: 'M10,10 C0,0 10,150 100,100 S300,150 400,50'
    // data: describeArc(0, 0, minDimension/3.5, initialAngleOffset + angle, initialAngleOffset + angle + facetAngleSize/2)
    // describeArc(x, y, radius, startAngle, endAngle),
    data: describeArc(
      0, 0, minDimension/3.45, 
      angle-facetAngleSize + (value.handle.length/2)*4,
      (angle+facetAngleSize)%360-facetAngleSize+(value.handle.length/2)*4
    )
  };
  graphData.push({
    handle: 'textArc_' + value.handle,
    configText,
    type: 'textArc'
  });
  console.log('graphData', JSON.parse(JSON.stringify(graphData)));
  angle += facetAngleSize;
}

graphData = JSON.parse(JSON.stringify(graphData));

console.log('graphData', JSON.parse(JSON.stringify(graphData)));

export default {
  name: 'app',
  components: {
    CircularGraph,
  },
  data: () => {
    return {
      graphData: graphData,
      reviewData: reviewData
    };
  }
}
</script>

<style>
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}

polygon {
    fill: #42b983;
    opacity: .75;
}

circle {
    fill: transparent;
    stroke: #999;
}

text {
    font-family: Helvetica Neue, Arial, sans-serif;
    font-size: 10px;
    fill: #666;
}

label {
    display: inline-block;
    margin-left: 10px;
    width: 20px;
}

#raw {
    position: absolute;
    top: 0;
    left: 300px;
}

</style>
