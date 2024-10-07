---
theme: dashboard
title: Fortune 500 Companies
toc: false
---

# Fortune 500 Companies & Reddit

```js
const f500 = FileAttachment("./data/f500reddit.csv").csv({typed: true});
```

```js
const color = Plot.scale({
  color: {
    type: "ordinal",
    domain: d3.groupSort(f500, (D) => -D.length, (d) => d.industry).filter((d) => d !== "Other"),
    unknown: "d3.InterpolateGreens"
  }
});
```

```js
function launchTimeline(data, {width} = {}) {
  return Plot.plot({
    title: "Dates founded",
    width,
    height: 300,
    color: {...color, legend: true},
    y: {grid: true, label: "Companies"},
    channels: {name: "company", industry: "industry", founded: "founded"},
    marks: [
      Plot.rectY(f500, Plot.binX({y: "count"}, {x: "founded", fill: "industry", interval: "year", tip: true})),
      Plot.ruleY([0])
    ]
  });
}
```

<div class="grid grid-cols-1">
  <div class="card">
    ${resize((width) => launchTimeline(f500, {width}))}
  </div>
</div>
