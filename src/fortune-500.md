---
theme: dashboard
title: Fortune 500 Companies
toc: false
---

# Fortune 500 Companies & Reddit
<span style="color: red;"><span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.</span>

```js
const f500 = FileAttachment("./data/f500reddit.csv").csv({typed: true});
```
## Table of data
```js
Inputs.table(f500);
```
## Visualizations

```js
function launchTimeline(f500, {width} = {}) {
  return Plot.plot({
    title: "Dates founded",
    width,
    height: 300,
    y: {grid: true, label: "Companies"},
    color: {...color, legend: true},
    marks: [
      Plot.rectY(f500, Plot.binX({y: "count"}, {x: "founded", fill: "industry"})),
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
