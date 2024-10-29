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

```js
Plot.plot({
  title: "When the companies were founded vs. when their brand ambassador's Reddit account was created",
  caption: "Figure 1",
  width: Math.max(width, 550),
  y: {
    domain: [1780, 2030],
    grid: true
  },
  x: {padding: 0.4},
  marks: [
    Plot.tip(data, Plot.pointerX({
      x: "rank",
      y: "founded",
      title: (d) => `${d.company} \n Founded: ${d.founded} \n Joined Reddit: ${d.redditorBrandCakeday}`
    })),
    Plot.dot(data, {
      x: "rank",
      y: "founded",
      dx: 2,
      dy: 2,
      r: 2
    }),
    Plot.dot(data, {
      x: "rank",
      y: "redditorBrandCakeday",
      fill: "green",
      dx: -2,
      dy: -2,
      r: 2
    }),
  ]
})
```

```js
Plot.plot({
  title: "Revenue change vs. ranking change, organized by industry",
  caption: "Figure 2",
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

