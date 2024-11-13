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
const industryFilter = view(
      Inputs.select(data.map(d => d.industry), {label: "Industry", sort: true, unique: true})
)
```

```js
Plot.plot({
  title: "When the companies were founded vs. when their brand ambassador's Reddit account was created",
  caption: "Figure 1",
  width: Math.max(width, 550),
  color: {legend: true},
  y: {
    domain: [1780, 2030],
    grid: true,
    label: "Date",
  },
  x: {
    grid: true,
    label: "Ranking"
  },
  marks: [
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
    Plot.crosshair(data, {
      x: "rank",
      y: "founded"
    }),
    Plot.tip(data, Plot.pointerX({
      x: "rank",
      y: "founded",
      fill: "white",
      title: (d) => `${d.company} \n Founded: ${d.founded} \n Joined Reddit: ${d.redditorBrandCakeday}`
    }))
  ]
})
```

```js
Plot.plot({
  title: "Revenues vs. membership in official subreddit",
  caption: "Figure 2",
  width: Math.max(width, 550),
  color: {legend: true},
  y: {
    grid: true,
    label: "Subreddit Members",
  },
  x: {
    grid: true,
    label: "Revenue (millions)"
  },
  marks: [
    Plot.dot(data, {x: "revenue", y: "subredditOfficialMembers", fill: "industry"}),
    Plot.crosshair(data, {
      x: "revenue", y: "subredditOfficialMembers",
    }),
    Plot.tip(data, Plot.pointerX({
      x: "revenue", y: "subredditOfficialMembers",
      fill: "white",
      title: (d) => `${d.company} \n Revenue ($M): ${d.revenue} \n Members: ${d.subredditOfficialMembers}`
    }))
  ]
})
```

```js
Plot.plot({
  title: "Revenue change vs. ranking change, organized by industry",
  caption: "Figure 3",
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

