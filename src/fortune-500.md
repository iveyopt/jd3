---
theme: dashboard
title: Fortune 500 Companies
toc: false
---
      
# Fortune 500 Companies & Reddit
<span style="color: red;"><span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.</span>

```js
const data = FileAttachment("./data/f500reddit.csv").csv({typed: true});
```

## Table of data

```js
Inputs.table(data, {rows: 20});
```

