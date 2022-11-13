# Google Lighthouse

## What is Google Lighthouse

One of the best automated tools available on web developer's utility belt.

It allows you to quickly audit websites in a number of key areas which together can inform a measure of it's overall quality.

- Performance
- Accessibility
- SEO
- Best Practices
- Progressive Web App

**Opportunities** in the results report should be definitely act upon.

Check the **Passed audits** for what you are already doing well in

---

## Running Lighthouse Audits

### Dev Tools

One of the most common ways to run Lighthouse Audits is directly through the Dev tool using either Chrome or Edge under the Audits panel.

In this instance, because the tests are running on your own machine rather that one of Google's own servers, conditions are not always the same between tests - which could lead skewed data sets.

Use dev tools Lighthouse more for **rough bulk auditing**.

A confirmation that Performance optimizations are working, rather than for long term performance monitoring.

---

### PageSpeed Insights

You can also run the Lighthouse audits from the web from the Google's PageSpeed Insights.

Provides field data sourced from a Chrome User Experience Report, a big data service providing metrics for how fast other Chrome users are loading the website

E.g. the 1.8s FCP represents the 75th percent tile for the metric, which is this is the FCP seen by users during the slowest 25% of observed page loads over the past 30 days.

---

### Google Analytics

A site speed is available under the behavior menu. Interested in the speed suggestions menu.

Here you get an overview of all the pages in the website and the respected average page load times.

Further along there is the PageSpeed Suggestions and and the PageSpeed score columns

These data points are pulled directly from Lighthouse via PageSpeed Insights.

---

## Lighthouse in Node.js

Allows you to:

- Command Line Usage
- Ouput JSON / CSV
- Testing Environment
- Block specific requests

Install lighthouse:

```
npm install lighthouse -g
```

Run a basic audit:

```
lightouse https://www.lukeharrison.dev --view
```

and have the reports automatically opened in the browser whens completed.

Generate a report and save it html json csv

```
lighthouse https://www.lukeharrison.dev --output json --output html
```

More advanced usage with the equivalent of a low mobile CPU and network condition slow 3G:

```
lighthouse https://www.lukeharrison.dev --throttling-method simulate --throttling.thoughtputKbps 512 --throttling.cpuSlowdownMultiplier 8 --view
```

---
