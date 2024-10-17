---
theme: dashboard
title: Fortune 500 Companies
toc: false
---

<script src="https://cdn.jsdelivr.net/npm/htl@0.3.1/dist/htl.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@observablehq/inputs@0.10.4/dist/inputs.js"></script>
      
# Fortune 500 Companies & Reddit
<span style="color: red;"><span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.</span>

```js
const f500 = FileAttachment("./data/f500reddit.csv").csv({typed: true});
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
