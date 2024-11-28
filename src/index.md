---
theme: dashboard
title: Fortune 500 Companies
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
</style>
      
# Fortune 500 Companies & Reddit
<span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.

```js
const data = FileAttachment("./data/f500reddit.csv").csv({typed: true});
```

```js
data
```

## Industry
500 companies means a lot of data! Select one or more industries from the list below to narrow the visualizations to only the data associated with those industries.
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
  marginTop: 100,
  marginRight: 100,
  marginLeft: 100,
  y: {
    domain: [1780, 2030],
    tickFormat: "",
    grid: true,
    label: "Date",
  },
  x: {
    grid: true,
    label: "Ranking"
  },
  marks: [
    //Draw the founded dates
    Plot.dot(data, {
      filter: (d) => industryFilter.includes(d.industry),
      x: "rank",
      y: "founded",
      fill: "black",
      r: 4
    }),
    //Draw the connecting line
    Plot.link(data, {
           filter: (d) => industryFilter.includes(d.industry),
           x1: "rank",
           y1: "founded",
           x2: "rank",
           y2: "redditorBrandCakeday",
           bend: "false",
           stroke: "black",
    }),
    //Draw the cake day dates
    Plot.dot(data, {
      filter: (d) => industryFilter.includes(d.industry),
      x: "rank",
      y: "redditorBrandCakeday",
      fill: "green",
      r: 4
    }),
    /*Plot.crosshair(data, {
      filter: (d) => industryFilter.includes(d.industry) && d.redditorBrandCakeday !== null,
      x: "rank",
      y: "founded"
    }),*/
    //Add the tooltips
    Plot.tip(data, Plot.pointer({
      filter: (d) => industryFilter.includes(d.industry),
      x: "rank",
      y: "founded",
      fill: "white",
      color: "black",
      title: (d) => `${d.company}  \n(#${d.rank}) \nFounded: ${d.founded} \nJoined Reddit: ${d.redditorBrandCakeday}`,
      fontSize: 16,
      anchor: "bottom",
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
      filter: (d) => industryFilter.includes(d.industry),
      x: "revenue",
      y: "subredditOfficialMembers",
      fill: "industry"
    }),
    Plot.crosshair(data, {
      filter: (d) => industryFilter.includes(d.industry),
      x: "revenue",
      y: "subredditOfficialMembers",
    }),
    Plot.tip(data, Plot.pointerX({
      filter: (d) => industryFilter.includes(d.industry),
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
