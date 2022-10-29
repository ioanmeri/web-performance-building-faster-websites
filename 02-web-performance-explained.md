# Web performance explained

## What is Web Performance

or WPO or Front End Performance is an area of web development concerned with making websites faster.

Not only for the initial loading to the browser, but for any user interaction or additional loading they are after.

**Web Performance Optimization**

- Lots of measuring
  - should be continues, as the website grows and develops, loading performance doesn't regress and only get's better
- Improving load times
  - largely done by performance optimization which reduce HTTP response latency
- Perceived performance

  - creating a perception that the website is loading faster that is actually is.

- Make sure images have alt tags
- Get Heather from marketing to sign off copy
- Web performance
- Add Google Analytics

Web performance is about accommodating users, looking after users, improving conversions, search engine optimization and perceived performance.

---

## Accommodating Users

The Internet is being accessed by users by many different devices types under many different network conditions.

There is the latest, greatest laptop mobile devices

- Dell XPS 13
- Apple iPhone 11 Pro

Budget and legacy devices

- Google Nexus 5
- Nokia 3310 3G
- Assorted Desktops

Devices that web usage is on the rise:

- Apple Watch 5

This means, websites with performance optimizations are going to offer very different user experiences, to those without.

**Example**

iPhone 8 loading CNN on 4G

- 4.5sec for the content to begin rendering
- 13sec until fully loaded

not amazing but it's usable

Moto G4 loading CNN On 3G

- 12.5sec for the content to begin rendering
- 51sec until fully loaded

If websites don't perform as expected, users will abandon the the website and just go elsewhere.

From studies, people believe:

> 50% of websites have remained the same speed or are even slower

> 71% feel that browsing regularly, disrupted by slow websites

> 78% feel negatively towards a slow or unresponsive website

> 44% believe a slow online transaction makes them doubt it was successful

> 42% decided not to use the website again because it was slow

Google discovered:

> Our research has been eye-opening. For 70% of the mobile landing pages we analyzed, it took more than five seconds for the visual content above the fold to display on the screen, and it took more than seven seconds to fully load all visual above and below the fold.

As page load time goes from

> 1s to 3s, the probability of bounce increases 32%

> 1s to 5s, the probability of bounce increases 90%

> 1s to 6s, the probability of bounce increases 106%

> 1s to 10s, the probability of bounce increases 123%

bouncing = leaving immediately

> BBC has seen that they lose an additional 10% of users for every additional second it takes for their site to load

WPO stats

> 53% of visits are abandoned if a mobile site takes longer than 3 seconds to load.

---

## Looking after users

The median mobile page weight, has steadily since 2011 from 150kb to 1.8Mb today (increase 1000% +)

The size of HTML on **mobile** has remained relatively consistent over the past decade (19kb to 26kb, 30% increase)

CSS from 12kb to 60kb (increase 400%)

Fonts from 57kb to 102kb (increase 79%)

JavaScript from 52kb to 391kb (increase 651%)

Images from 100kb to almost 1Mb (increase 775%)

### Why this matter when lots of people have multi Gb data bands

- Many countries have Pay-as-you-go (PAYG) system

- Small data plans

- International roaming charged by mb

In addition to the cost factor, websites such as government information portals, hospitals and crisis centers, must absolutely serve a fast user experience.

Moral responsibility to look after users by optimizing our websites performance

Don't burden users with **unnecessary costs** or **delayed access** to important information.

---

## Search Engine Optimization

Since 2010, Google has considered speed a ranking factor when calculating organic listing on desktop.

Later, in 2018, Google announced they would extend this ranking factor to searches run on mobile.

A slow loading website can negatively impact SEO and lead to lower organic rankings on search engine results pages.

organic = not ad driven

Did it work? Well, yes.

- Slowest 1/3 of websites got 15 / 20 % faster

- Improvements in 95% of countries.

- 20% less abandonment for navigation from search

When companies focus on web performance, they see improvements to organic traffic.

> Rebuilding Pinterest pages for performance resulted in a 40% decrease in wait itme, a 15% increase in SEO traffic and a 15% increase in conversion rate to signup.

> Carousell reduced page load time by 65% and saw a 63% increase in organic traffic, a 3x increase in advertising click-thru and a 46% increase in first-time chatters.

---

## Improving Conversions

An e commerce business it's goal is to generate products sales

For an agency offering designing consultation services to other business it's goal is to generate interest, and ultimately leads.

For a government the goal should be to provide access to accurate information and systems for citizens.

Every website has goals and when a goal is met, that's generally **what's called a conversion**

The more conversions the business generates, the more revenue it generates.

When users first visit your website it's a critical time in the sales journey. Their experiences here, good or bad will impact the chances that generate a conversion.

The first load, it's an uncached one. So every asset must be downloaded by the browser in full. This means **a user is experiencing a website load for the first time, under the worst possible conditions**.

So if

- the hero images don't display fast enough
- the page is constantly jumping around, as elements loaded in
- call to action buttons aren't interactive when the user first clicks

critical information may be missed and conversion opportunities may be lost.

> Only 2% of the users convert on the first visit

This means, it's important the repeated visits are loading experiences even faster with:

- cache
- service workers

> 50% experience slowdown during traffic, of those 82% have had website access prevented

> 50% will abandon, 1 in 5 users won't return

Radware while conducting a study found that if pages aren't fast, everything suffers.

- Content: "boring"
- Visual design "tacky" and "confusing"
- Navigation "frustrating" and "hard-to-navigate"

Overall, if a website:

- Loads fast
- Responsive to user input
- Usable on many device types & network conditions

then it provides a:

- Reliable, positive experience
- Doesn't block conversions
- Competitive advantage

> Ancestory.com saw a 7% increase in conversions after improving render time by 68%, page weight by 46% and load time by 64%.

> Staples reduced median homepage load time by 1 second and reduced load time for th 98th percentile by 6 seconds. As a result, they saw a 10% increase in their conversion rate.

So the relationship between web performance and site performance is pretty clear. The faster your website, the more revenue you can expect to generate.

---

## Perceived performance

When it comes to web performance, the user's perception of how fast the website loads can be more important than how fast it actually loads.

**Performance isn't about mathematics**

**Performance is about perception**

**Example**

if Objective Load time of amazon is 30 seconds

and Psychological Load Time is 5 seconds

then the latter psychological metric overrides the former objective one.

The customer is always right

This is called optimizing the perceived performance.

The human brain perceives time in a different way from a clock, which tracks what we'd call objective.

It can fluctuate depending on metrics such as age:

- age
- anxiety levels
- time of day

this is called **Psychological Time**

There are two typical phases:

- Active Phase
  - is a period of await where the user's mind is engaged, e.g. while riding a bike, solving a puzzle, watching TV, or thinking in depth about something
- Passive Phase
  - characterized by the opposite, periods of inactivity where the user isn't engaged and has no choice or control of awaiting times, e.g. waiting for a train to arrive, waiting for your luggage at the airport, waiting for the bill at a restaurant

Users don't like to wait and when they complain that is taking to long this generally refers to the length of the passive wait.

There is two main techniques to increase the active phase:

- Pre-emptive Start
  - which involves beginning the wait in the active phase and maintaining it for as long as possible before switching to the passive phase
  - this way even if though the total wait times remain the same, the users perception of the wait time is reduced, because for the first phase of the wait they where occupied
- Early Completion
  - turning a passive wait into an active wait, as soon as possible, by giving the users something to engage with

In addition to improving the objective performance of a website, we can also improve it's perceived performance.

---
