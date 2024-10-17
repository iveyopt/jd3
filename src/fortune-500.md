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
<!-- A shared color scale for consistency, sorted by the number of launches -->

```js
const color = Plot.scale({
  color: {
    type: "categorical",
    domain: d3.groupSort(f500, (D) => -D.length, (d) => d.industry).filter((d) => d !== "Other"),
    unknown: "var(--theme-foreground-muted)"
  }
});
```

## Table of data
```js
Inputs.table(f500);
```
