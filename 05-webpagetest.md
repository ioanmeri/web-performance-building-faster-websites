# WebPageTest

## Running WebPageTest Audits

A synthetic testing tool used to measure the performance of a website and capture a tone of lab data.

Options available when configuring the test:

- Test location
- Browser
- Repeat view
  - records how the website performs on subsequent visits.

**Test Settings**

- Connection
  - lets you throttle the network speed
- Number of Tests Run
  - recommended 5 tests, this way running multiple tests under the same conditions establishes what is consistently good / bad.
- Label
  - to identify later on history screen

**Advanced**

Can be used for e.g.

- Ignore SSL Certificate Errors
  - useful when testing locally or in development website
- Disabling JS
  - could be used to test the impact JavaScript bundle sizes have on a website
- Save response bodies
  - allow you to view the source of each requested text file
- User Agent String
  - could be used for tracking
- Inject script
  - could be used to trigger hidden modal, or complete a form with specific data, before submitting it.

**Chromium**

Specific to Chromium based browsers like Chrome and Edge

- Capture LightHouse reports

- Emulate mobile browser

**Auth**

Test the performance of specific journeys which may involve accessing content behind a logging screen

**Script**

Programmatically declare how a webpage test should interact with the web page

e.g. an entire sales journey consisting of multiple pages

**Block**

Can tell a webpagetest to block requests which meets criteria

- test the performance impact of 3rd party scripts such as ads for example

**SPOF**

Single Point of Failure

Configure a web page to force a specific request to timeout which could be used to simulate an outage

**Custom**

Allows you to execute arbitrary JS as the end of the test

- collect custom metrics specific to your own goals

**Bulk**

Only if you are logged in and append to URL `/?bulk=1`

Can run multiple tests under the same conditions

---

## WebPageTest Reports

It's report is a permalink

**Report Scorecard**

At the top of each page of the report WebPageTest displays a high level score card which summarizes website performance in various categories.

- Performance aggregate number from Google Lighthouse
- Security score
  - results of JS vulnerability and security header audit
  - headers missing e.g. strict transport security, Content Type Options, Content Security Policy
- First Byte Time
  ...

---

**Summary**

- Performance Results (Median Run - SpeedIndex)
- Timeline
- Screenshots
  - First View
  - Repeat View

---

**Details**

Builds upon information found in summary. The key piece of information is the waterfall.

The most important part of the report, because it visualizes the website's network activity.

Using this you can identify:

- Slow Resources
- Blocking Resources
- 3rd party resources which if slow or broken will severely impact the loading of the website. **SPoF requests**
- How well **HTTP/2** is being utilized to load resources simultaneously

In addition, there is a

- film strip page
- Connection View
- Requests Details

---

**Performance Review**

A super granular checklist of what optimizations can be detected on the website

- GZip Enabled?
- Images Compressed?
- Jpgs Progressive?
- Efficient Caching?
- Static Assets CDN?

---

**Content Breakdown**

Breaks down different types of content loaded for both the first and repeat views. It split into the number of request per type and then the amount of types per byte, both by percentage.

This allows to identify assets which are disproportionally large.

---

**Domains**

Organizes requests by domain for both first and repeat view. Bad third party scripts which slow down your page load, will probably be exposed here. May have a lots of requests or few requests will lots of bytes

---

**Main-thread Processing Breakdown**

How the browser is handling the loading and rendering of your website under the hood

Useful when working with SPAs because these are client-side frameworks, they use more browser resources

---

**Screenshot**

Key screenshots.

- Fully Loaded
- Hero Time: Image (if you've configured WebPageTest)
- Hero Time: Heading (if you've configured WebPageTest)

---

**External Links**

**Image analysis** opens this external report which is a more in-depth look into the websites images and how many bytes can potentially be saved if they were

- compressed
- saved in more appropriate file format

---

**Request Map**

Visualizes 3rd party requests, and for it's of this the requests they fire off

Useful for non technical stakeholders to help them visualize the issue if the website has too many 3rd party scripts

E.g. marketing department abusing Google tag manager for instance

---

**Comparing Reports**

WebPageTest makes comparison easy to compare reports functionality. Can generate 2 reports for example in one 4g and the other 3g to simulate the difference in performance.

Click the test history and then find the 2 reports, select them both and click Compare.

You can compare the reports in a number of ways:

- Loading of both reports in film strip
  - and export as image to share with stakeholders
- Generate video comparison
  - useful for non technical stakeholders

and finally there is the hard data, useful for developers and identifying more subtle differences between reports

---

**WebPageTest in Summary**

Due to the amount of information you are presented with, WebPageTest can be overwhelming at first.

However, once you over it's admittedly steep learning curve you'll come to see it as a very powerful tool which help you measure and improve the performance of your website

---
