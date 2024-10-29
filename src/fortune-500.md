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

## When the companies were founded vs. when their brand ambassador's Reddit account was created
```js
Plot.plot({
  width: Math.max(width, 550),
  y: {
    domain: [new Date("1780-01-01"), new Date("2030-01-01")],
    grid: true
  },
  x: {padding: 0.4},
  marks: [
    Plot.dot(data, {x: "rank", y: "founded", dx: 2, dy: 2}),
    Plot.dot(data, {x: "rank", y: "redditorBrandCakeday", fill: "green", dx: -2, dy: -2})
  ]
})
```

## Revenue change vs. ranking change, organized by industry

```js
Plot.plot({
  width: Math.max(width, 550),
  grid: true,
  inset: 10,
  color: {legend: true},
  marks: [
    Plot.frame(),
    Plot.dot(data, {x: "revenueChange", y: "rankChange", stroke: "industry"})
  ]
})
```

