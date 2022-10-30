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
