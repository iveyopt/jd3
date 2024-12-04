---
theme: dashboard
title: Figure 1
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

```js
Plot.plot({
  title: "Figure 1: Founding Dates vs. Cake Days",
  subtitle: "Black dots indicate companies' founding dates, and green dots indicate the 'Cake Day' for the company's brand ambassador's Reddit account (that is, the date the account was created). Black dots without a connected green dot indicate that the company has no Reddit ambassador account. Hover over a dot to show the company info!",
  width: Math.max(width, 550),
  style: "overflow: visible",
  marginRight: 100,
  marginLeft: 100,
  marginBottom: 50,
  y: {
    domain: [new Date("1770-01-01T00:00:00.000Z"), new Date("2040-01-01T00:00:00.000Z")],
    grid: true,
    label: "Date",
    labelAnchor: "center",
  },
  x: {
    grid: true,
    label: "Fortune 500 Ranking",
    labelAnchor: "center",
  },
  marks: [
    //Draw the founded dates
    Plot.dot(data, {
      filter: (d) => industryFilter.includes(d.industry),
      x: "rank",
      //y: "founded",
      y: (d) => parseTime(d.founded),
      fill: "black",
      r: 4
    }),
    //Draw the connecting line
    Plot.link(data, {
           filter: (d) => industryFilter.includes(d.industry),
           x1: "rank",
           y1: (d) => parseTime(d.founded),
           x2: "rank",
           y2: (d) => parseTime(d.redditorBrandCakeday),
           bend: "false",
           stroke: "black",
    }),
    //Draw the cake day dates
    Plot.dot(data, {
      filter: (d) => industryFilter.includes(d.industry),
      x: "rank",
      y: (d) => parseTime(d.redditorBrandCakeday),
      fill: "green",
      stroke: "black",
      r: 4
    }),
    /*Plot.crosshair(data, {
      filter: (d) => industryFilter.includes(d.industry) && d.redditorBrandCakeday !== null,
      x: "rank",
      y: (d) => parseTime(d.founded),
    }),*/
    //Add the tooltips
    Plot.tip(data,
     Plot.pointerX({
      filter: (d) => industryFilter.includes(d.industry),
      x: "rank",
      y: (d) => parseTime(d.founded),
      fill: "white",
      color: "black",
      title: (d) => 
      (d.redditorBrandCakeday !== null) ?
          `#${d.rank}  \n${d.company} \nIndustry: ${d.industry} \nFounded: ${d.founded} \nJoined Reddit: ${d.redditorBrandCakeday} (${d.redditorBrandCakeday - d.founded} years later)`
      :
          `#${d.rank}  \n${d.company} \nIndustry: ${d.industry} \nFounded: ${d.founded} \nNo Reddit account`,
      fontSize: 16,
      anchor: "bottom",
    }))
  ]
})
```
