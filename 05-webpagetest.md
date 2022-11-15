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
