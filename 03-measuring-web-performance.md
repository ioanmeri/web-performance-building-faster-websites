# Measuring Web Performance

## How Browsers Load Websites

### Navigation

If it's the first time the user is visiting the page, the browser needs to know the location of the web server

### DNS Lookup

The DNS Lookup is handled by lukeharrison.dev name servers. Which response to this request with an IP address `77.72.0.110`, the location of the server storing website's files.

This IP then will be cached from the browser for a time.

DNS Lookups happen for each unique domain and have a cost. For example, if you have assets such as CSS or JS **stored on a CDN**, **each of these domain names will require their own DNS Lookup**

### TCP Handshake

Now that the browser knows IP address of the server, it can connect.

This is done with what is called a TCP handshake, which negotiates a connection between the user's browser and the website's server. So that later can be used sent and received freely.

### TLS Handshake

A TLS Handshake is required to establish a secure connection with the website's server.

### HTTP Response

```
GET /index.html HTTP/2.0
Host: lukeharrison.dev
```

Response

```
HTTP/2.0 200 OK
Response <html>...</html>
```

The journey to the server and then back again with the response is called a **Roundtrip**

### Critical Rendering Path

Refers to the minimum number of steps which the browser must complete once it begins to receive assets from the server, and convert a website's HTML, CSS and JavaScript into screen pixels.

DOM > CSSOM > Render Tree > Layout > Paint

Optimizing the Critical Rendering Path, so it completes faster, is one of the main ways to speed up the loading of a website.

**DOM**

The browser parses the HTML returned in the first request and uses it to generate the DOM

As the HTML parser creates new nodes, some will contain link to external resources.

If it's a link element referencing a CSS stylesheet, the browser fires up a request to retrieve it, and HTML parser continues

```
GET /style.css HTTP/2.0
Host: lukeharrison.dev
```

If it finds a script element referencing a JavaScript file, unless the element it contains a **defer** or **async** attribute, the **HTML parser pauses** until the request is completed

```
<script src="script.js></script>
```

```
GET /script.js HTTP/2.0
Host: lukeharrison.dev
```

```
HTTP/2.0 200 OK
Response: function(){..}
```

Once the script is downloaded, and only if no CSS requests are currently in flight, the script is executed.

The parser the resumes until runs out of HTML.

Once the HTML is fully parsed, and the DOM is complete, the browser fires the **DOMContentLoaded** event.

**CSSOM**

Contains all of the page's CSS styles.

By default, the browser won't progress it's page render until it has completed the construction of the CSSOM, as if it didn't it would render an inaccurate picture of the website, and it would get a flash of unstyled content.

This behavior is why CSS is considered render blocking and a part of the critical rendering path.

**Render Tree**

DOM + CSSOM = Render Tree

which is a representation of the page's styled visible content.

The browser will use this to render the content to the screen.

**Layout**

The browser now can calculate where and how elements should be positioned relative to each other, on the screen. This is called the layout phase.

How fast this process complete is impacted by the size of the DOM.

The more nodes the DOM contains, the more calculations this layout phase will have to run to position everything correctly.

Also, whenever the DOM is modified, for example if the user creates new elements or editing the content of existing one using JS, the positions of affected elements must be updated accordingly.

This repeat layout is called a **reflow**, and can be expensive from a resource perceptive.

This is because a reflow is triggered, further reflows are also triggered on the elements children and ancestors and also elements that appear after in the DOM.

Basically, a chain reaction should be avoided.

**Paint**

On first load, the entire screen is painted, beyond that, only areas impacted by layout changes will undergo re-paint.

Without this optimization, the smallest change will use the same amount of resources as the biggest, which isn't very efficient

**Interactivity**

Now that all the content has been rendered on the screen, all that remains to execute is JavaScript with **defer** on script tag, which is programmed to listen out for the browsers on load event

```
<script src="script.js" defer></script>
```

```
window.addEventListener("load", (event) => {
  console.log('page is fully loaded');
})
```

which if fired once the page has been rendered and any external resources have been loaded.

---

## About Measuring Web Performance

Can split it into to big categories

- Loading Performance

- Rendering Performance

**Loading Performance**

Is the area of web performance optimization, focusing around optimizing the delivery of assets from a server to the browser.

Concerted with:

- File sizes
- Asset Load Order
- No Unused Assets/code
- No Blocking Scripts

**Rendering Performance**

Focuses on optimizing how the browser uses the downloaded assets to render the website and handle any sub sequent interactions.

- CSS Style Calculations
- Unnecessary Reflows
- Bad Event Listeners

### Measuring Web Performance

can use:

- Lab Data
- Field Data

### Lab Data

- Captured using synthetic testing
- In a controlled, simulated environment
- Full control over device type, network conditions etc
- e.g. Google Lighthouse, Webpagetest

#### Advantages

- Captures lots of data
- Can measure websites still in development
- Can measure competitors
- Prevents regressions

#### Disadvantages

- Doesn't capture the complete picture, only snapshots
- Unable to directly compare against other data points

### Field Data

- Data collected from real users
- AKA: Real User Monitoring (RUM)
- e.g. Google Analytics
- Captures the real world user experience, quirks and all

#### Advantages

- Monitoring performance during high traffic events
- Collects user data
  - browser, device, OS, location
- Can directly cross-reference web performance against other data points
  - bound rate, page views, time on site, conversion rate

#### Disadvantages

- Not suited for debugging performance issues
- Can't benchmark against competitors
- Can't effectively measure websites in development

### Factors which impact page load

- User's Location
- Network Traffic
- Network Speed
- Device Power

Web performance is a collection of all users loading experiences.

---

## Loading Performance Metrics

When measuring Web Performance there are many different types of metrics we can track:

- Time to First Byte
- First Paint, First Contentful Paint
- Speed Index, Largest Contentful Paint
- Time to Interactive, Time to First CPU Idle
- Total Blocking Time
- First Input Delay
- Load Time
- Cumulative layout Shift

These all generally fall within the following categories:

- **Quantity-Based** Metrics
- **Milestone** Metrics

### Quantity Based Metrics

Measure things such as:

- the number of HTTP requests
- the size of the page in bytes
- the number of images loaded

basically anything you can count, used for long-term monitoring and reveal trends or highlights problems for 3rd party scripts for example.

### Milestone Metrics

Measure how long it takes the browser to reach a specific phase of loading process, for instance:

- how long the server takes to respond
- how long it takes the browser to start rendering content
- how long it takes for that content to become interactive, when JS is completed.

helps us earn more about overall user experience, require a little more elaboration.

---

## Time to First Byte (TTFB)

Used to measure the responsiveness of the web server. How long it takes the web server to respond to the browser's first request.

Bad server config or backend coding can be responsible for any poor performance in this area.

Also, the further you are from the website server geographically, the longer it takes to respond.

---

## First Paint / First Contentful Paint

AKA Start Render measures how long all of the Critical Rendering Path takes to complete.

How long it takes between when the user types in the web address and presses enter to when bits of website content, however small, first begin to appear in the browser window.

**First Contentful Paint**

Like First Paint, it measures when the first bits of content are rendered to the screen but makes a distinction that the content must be useful to the user.

This means for the metric to trigger, the new content must be

- text
- images (or background-image)
- svg
- canvas (non white)

---

## Speed Index / Largest Contentful Paint

In the past, it is been common to track a website speed by measuring when the browser fires events such as DOMContentLoaded and load.

These metrics aren't a very good indicator of the actual end user experience

If the browser has visible content in the view port then the user can meaningfully interact with the website and doesn't have to wait for the DOMContentLoaded the whole page.

Speed Index tries to fakes this inaccuracy by measuring the average time it takes for only the viewport content to be rendered, with any below the fold elements now safely ignored.

This means the mobile devices will always have a lower speed index, simply because the is less viewport content to render.

**Largest Contentful Paint**

Similar to speed index, identify when the main content of the page has loaded

Looks when the largest image, or text block is visible within the view port.

---

## Time to Interactive / Time to First CPU Idle

Measures the point in the loading process, where the page is fully interactive.

The page must be complete enough to facilitate interaction, this means the page must be displaying useful content which were tracking using FCP.

When the page is **FULLY** interactive

**Time to First CPU Idle**

AKA First Interactive

When the page is **FIRST** interactive

---

## Total Blocking Time

Another JS oriented performance metric

It measures the amount of **time** between **between First Contentful Paint** and the **Time To Interactive**

where the browser was too busy executing JavaScript to accept input

---

## First Input Delay

or Input Latency, represents the time from when the user is first able to interact with elements on the page to the point when these elements are able to respond to those interactions.

Which depending on what the browser is doing isn't always immediately.

This is because browsers are single treaded.

So, if page content has finished rendering, or the browser is still busy executing JS, any user input during this time will not be registered, as the browser doesn't have the resources available to respond

Poor First Input Delay = User's frustration

Interaction snappier or unresponsive.

---

## Load Time

depending on the tool can be:

- Browser onLoad event fired
- onLoad event fired + no more network activity

less important because it is not an accurate representation of the users own experience.

E.g.

Speed Index: 3sec

Time to Interactive: 5sec

Load Time: 15sec

The website is usable at 5sec not 15sec

---

## Cumulative Layout Shift

Often when the website is loading, during the render phase, the layout of the page can suddenly shift as new elements re introduced.

Very frustrating for a user trying to interact with the page.

Cumulative Layout Shift measures these unstable elements and how often they trigger unnecessary reflows.

The lower the score the better.

---

## Performance Budgets

Once optimization is put into place, it's important to prevent it to regression. This can be done through establishing what's called a **Performance Budget**

A Performance Budget is the idea that for each metric, there is a threshold which you cannot fall below.

What should the Performance Budget be??

Most businesses aren't ready for challenges, bur rather need safeguards.

Performance budget metrics should be: Whatever the live website performance metrics are right now.

Metrics should never get worse, only better.

By setting your performance budget for each metric to be the worse it's been over the last 2-week period, you creating a safety net, to keep the website from progressing further.

| Metric        | First paint | First Meaningful Paint | Speed Index | # 3rd party requests |
| ------------- | ----------- | ---------------------- | ----------- | -------------------- |
| 6 Weeks Ago   | 4.0s        | 4.6s                   | 5.2s        | 50                   |
| 4 Weeks Ago   | 3.8s        | 4.4s                   | 5.0s        | 46                   |
| 2 Weeks Ago   | 4.1s        | 4.5s                   | 5.5s        | 55                   |
| Current Worst | 2.9s        | 3.3s                   | 4.0s        | 37                   |
| Budgeted      | 3.8s        | 4.4s                   | 5.0s        | 46                   |

What happens if the metrics have gotten worse?

The the performance budget stays the same and you put in the work to get it back on track

Following this method, you will find yourself making slow incremental improvements to the performance of your website. Whilst making sure that you or any other developer don't make things worse.

---
