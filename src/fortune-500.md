---
theme: dashboard
title: Fortune 500 Companies
toc: false
---
<script src="https://d3js.org/d3.v4.js"></script>
      
# Fortune 500 Companies & Reddit
<span style="color: red;"><span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.</span>

```js
const data = FileAttachment("./data/f500reddit.csv").csv({typed: true});
```

```js
data
```

## Sampledata
```js
Plot.rectY({length: 10000}, Plot.binX({y: "count"}, {x: d3.randomNormal()})).plot()
```

## When the companies were founded vs. when their brand ambassador's Reddit account was created
```js
Plot.plot({
  height: 2050,
  width: 5000,
  marginTop: 20,
  marginRight: 20,
  marginBottom: 30,
  marginLeft: 40,
  grid: true,
  x: {padding: 0.4},
  marks: [
    Plot.barY(data, {x: "rank", y: "founded", dx: 2, dy: 2}),
    Plot.barY(data, {x: "rank", y: "redditorBrandCakeday", fill: "green", dx: -2, dy: -2})
  ]
})
```

## Revenue change vs. ranking change, organized by industry

```js
Plot.plot({
  grid: true,
  inset: 10,
  color: {legend: true},
  marks: [
    Plot.frame(),
    Plot.dot(data, {x: "revenueChange", y: "rankChange", stroke: "industry"})
  ]
})
```

