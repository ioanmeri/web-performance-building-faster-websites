# Other Performance tools

## Basic RUM with Google

Google analytics captures Real Use Measurement (RUM) on site speed and other related metrics based on 1% data sample.

An excellent source of free field data.

**Behavior** > **Site Speed** > **Page Timings**

A few useful comparisons which may yield meaningful results are:

- Compare average server response time
- against average document interaction time

If there is big goal between the two that may indicate that there is too much JavaScript executing on first load

- bounce rates
- against average page load time

If bounce rates is up, and so is average page load time this suggest a link between these two variables

- average server connection time
- average server response time

A big difference would suggest inefficient server side code which affects server response times

If you are tracking sales over Google analytics, you could overlay conversion against page speed and see if it takes a hit during periods where the website loads especially slow.

Alternatively you could track opposite. As page speed improves, is this having a tangible effect on sales.

Also, very important to filter geographically as factoring in data from countries you aren't targeting may skew your insights.

**Distribution tab** > Same data in more detail.

---

## Simulating Network Speeds

In Chrome dev tools > **Network Tab** > Simulate 4G 3G etc

Keep dev tools open for the throttling to stay active

Firefox is pretty similar to Chrome (but you can't define your own custom speeds).

Because DevTools throttling is implemented in the browser level, which is software, it sits well above the part of the OS which handles the network. This means the dev tools throttling won't be fully accurate and should only be used for rough bulk timings.

On **Mac** use the utility Network Link Conditioner

In **Windows**, download Charles proxy

in **iOS simulator** again with Network Link Conditioner

in real **iOS device**, connect to Mac by USB, in iOS device click on Settings > Developer > Network Link > Conditioner which allows you to throttle your iOS device to one of those preset speeds.

on **Android** download BradyBound, requires to root your android device. Alternative if you have a cellular data plan can change to 4G to 3G and throttle your data too, or if you don't have a SIM card can share your Mac or PC as a WiFi Hotspot.

---

## Advanced Throttling & SPOF Testing

### Scripts Loading Slow

If more targeted throttling is required, for example testing how the website runs when only a specific 3rd party resource is **loading slow**.

Can do all of this with Charles, fully fetched proxy application

Proxy > Throttle settings > Add and paste jQuery URL

This significantly slows down the rendering of the website as links that don't have `defer` or `async` attribute are retread as blocking by the browser.

The critical rendering path cannot complete until these are downloaded and executed.

### Scripts don't load at all

Sometimes resources, **don't load at all**

Tools > DNS Spoofing

Redirect any host to a different IP address

Single Point of Failure Testing

---
