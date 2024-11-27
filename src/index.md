---
theme: near-midnight
title: Fortune 500 Companies
toc: false
---
<script src="https://d3js.org/d3.v4.js"></script>
<style>
      * {
            font-family: Roboto;
      }
</style>
      
# Fortune 500 Companies & Reddit
<span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.

```js
const data = FileAttachment("./data/f500reddit.csv").csv({typed: true});

```

```js
data
```

## Select an industry:
500 companies is a lot! Select an industry from the dropdown below to view only the data associated with that industry. (Currently, this tool does not include the ability to compare across multiple industries.)
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
  y: {
    domain: [new Date("1780-01-01"), new Date("2030-01-01")],
    grid: true,
    label: "Date",
  },
  x: {
    grid: true,
    label: "Ranking"
  },
  marks: [
    Plot.link(
      data,
      Plot.groupX(
        {
          y1: (D) => d3.sum(D, (d) => d === "founded"),
          y2: (D) => d3.sum(D, (d) => d === "redditorBrandCakeday"),
          stroke: (D) => d3.sum(D, (d) => d === "redditorBrandCakeday") - d3.sum(D, (d) => d === "founded")
        },
        {
          x: "rank",
          y1: "founded",
          y2: "redditorBrandCakeday",
          markerStart: "dot",
          markerEnd: "arrow",
          strokeWidth: 2
        }
      )
    ),
    Plot.dot(data, {
      filter: (d) => d.industry == industryFilter,
      x: "rank",
      y: "founded",
      dx: 4,
      dy: 4,
      r: 4
    }),
    Plot.dot(data, {
      filter: (d) => d.industry == industryFilter,
      x: "rank",
      y: "redditorBrandCakeday",
      fill: "green",
      dx: -4,
      dy: -4,
      r: 4
    }),
    /*Plot.crosshair(data, {
      filter: (d) => d.industry == industryFilter,
      x: "rank",
      y: "founded"
    }),*/
    Plot.tip(data, Plot.pointerX({
      filter: (d) => d.industry == industryFilter,
      x: "rank",
      y: "redditorBrandCakeday",
      fill: "white",
      title: (d) => `${d.company} \n Founded: ${d.founded} \n Joined Reddit: ${d.redditorBrandCakeday}`,
      fontSize: 16,
      fontFamily: "Roboto",
      
      anchor: "top",
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
    Plot.dot(data, {
      filter: (d) => d.industry == industryFilter,
      x: "revenue",
      y: "subredditOfficialMembers",
      fill: "industry"
    }),
    Plot.crosshair(data, {
      filter: (d) => d.industry == industryFilter,
      x: "revenue",
      y: "subredditOfficialMembers",
    }),
    Plot.tip(data, Plot.pointerX({
      filter: (d) => d.industry == industryFilter,
      x: "revenue", y: "subredditOfficialMembers",
      fill: "white",
      title: (d) => `${d.company} \n Revenue ($M): ${d.revenue} \n Members: ${d.subredditOfficialMembers}`
    }))
  ]
})
```

```js
const companyFilter = view(
      Inputs.select(data.map(d => d.company), {label: "Company", sort: true, unique: true})
)
```

The section below is a not-working-yet plot where I'm trying to get a bar chart of the employee subreddit membership number layered over the total employees number as 100% — so if one place has 50 employees in the subreddit out of 100 total employees and another place has 25 employees in the subreddit out of 50 total employees, they both show as 50% filled. It isn't working yet and needs some help. 

```js
Plot.plot({
  title: "Percent of employees present in employee subreddit",
  caption: "Figure 3",
  width: Math.max(width, 550),
  grid: true,
  inset: 10,
  color: {legend: true},
  y: {
    grid: true,
    label: "Subreddit Members",
  },
  x: {
    grid: true,
    label: ""
  },
  x: {
    axis: "top",
    grid: true,
    percent: true
  },
  marks: [
    Plot.barY(data, {
      filter: (d) => d.industry == industryFilter && d.subredditEmployeeMembers !=== null,
      x: "employees",
      y: "subredditOfficialMembers",
      fill: "industry"
    }),
    Plot.crosshair(data, {
      filter: (d) => d.industry == industryFilter,
      x: "employees",
      y: "subredditEmployeeMembers",
    }),
    Plot.tip(data, Plot.pointerX({
      filter: (d) => d.industry == industryFilter,
      x: "employees", y: "subredditEmployeeMembers",
      fill: "white",
      title: (d) => `${d.company} \n Employees ($M): ${d.employees} \n Members: ${d.subredditEmployeeMembers}`
    }))
  ]
  marks: [
    Plot.ruleX([0]),
    Plot.barX(alphabet, {x: "frequency", y: "letter", sort: {y: "x", reverse: true}})
  ]
})
```

<!-- ```js
Plot.plot({
  title: "Revenue change vs. ranking change, organized by industry",
  caption: "Figure 0",
  width: Math.max(width, 550),
  grid: true,
  inset: 10,
  color: {legend: true},
  marks: [
    Plot.frame(),
    Plot.dot(data, {
      filter: (d) => d.industry == industryFilter,
      x: "revenueChange",
      y: "rankChange",
      stroke: "industry"})
  ]
})
``` -->
