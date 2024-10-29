---
theme: dashboard
title: Fortune 500 Companies
toc: false
---
<script src="https://d3js.org/d3.v4.js"></script>
      
# Fortune 500 Companies & Reddit
<span style="color: red;"><span style="text-transform: uppercase; font-style:italic">Disclaimer:</span> Parts of this data were AI-generated. The data used in this project has not been verified and may not be correct.</span>

```js
console.log('d3', d3.version);
document.getElementByID('d3version')[0].innerText = d3.version;
```
<p>D3 Version: <span id="d3version"></span></p>

```js
const data = FileAttachment("./data/f500reddit.csv").csv({typed: true});
```

```js
data
```

## Table of data

```js
Inputs.table(data, {rows: 20});
```

