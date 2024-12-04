---
theme: dashboard
title: Figure 2
toc: false
---
<script src="https://d3js.org/d3.v4.js"></script>
<style>
      * {
            font-family: Roboto;
      }
     p, h1, h2, h3, h4, h5, h6 {
          max-width: 100%;
          text-wrap: auto;
     }
     button, input, textarea {
          accent-color: green;
     }
     g>text {
         font-size: 16px;
     }
     form>label {
         width: 200px !important;
     }
</style>
      
```js
const data = FileAttachment("./data/f500reddit.csv").csv({typed: true});

//Parser for dates
const parseTime = d3.utcParse("%Y");
```

### Select One or More Industries to View
```js
const industryFilter = view(
  Inputs.checkbox(
    data.map((d) => d.industry),
    {
     value: ["Technology"],
     label: "Select industries",
     sort: true,
     unique: true,
     multiple: true,
    }
  )
)
```

### Figure 2: Revenues vs. membership in official subreddit
Use the sliders below to zoom the second chart — some companies have way more subreddit members than others!

```js
const y_max = view(Inputs.range([2000, 6000000], {value: 6000000, label: "Maximum chart height (Members)"}));
const x_max = view(Inputs.range([500, 700000], {value: 700000, label: "Maximum chart width (Revenue)"}));
```

```js
Plot.plot({
  marginTop: 50,
  marginBottom: 50,
  marginRight: 100,
  marginLeft: 100,
  width: Math.max(width, 550),
  style: "overflow: hidden",
  y: {
    grid: true,
    label: "Subreddit Members",
    labelAnchor: "center",
    domain: [1, y_max],
  },
  x: {
    grid: true,
    label: "Revenue (millions)",
    labelAnchor: "center",
    domain: [1, x_max],
  },
  marks: [
    Plot.dot(data, {
      filter: (d) => industryFilter.includes(d.industry) && d.subredditOfficialMembers !== null,
      x: "revenue",
      y: "subredditOfficialMembers",
      fill: "green",
      stroke: "black",
      r: 4,
    }),
    Plot.crosshair(data, {
      filter: (d) => industryFilter.includes(d.industry) && d.subredditOfficialMembers !== null,
      x: "revenue",
      y: "subredditOfficialMembers",
    }),
    Plot.tip(data, Plot.pointerX({
      filter: (d) => industryFilter.includes(d.industry) && d.subredditOfficialMembers !== null,
      x: "revenue", y: "subredditOfficialMembers",
      fill: "white",
      color: "black",
      title: (d) => `${d.company} \nIndustry: ${d.industry} \nRevenue ($M): ${d.revenue} \nMembers: ${d.subredditOfficialMembers}`,
      fontSize: 16,
      anchor: "bottom",
    }))
  ]
})
```
