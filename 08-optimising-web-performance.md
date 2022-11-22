# Optimizing Web Performance

## HTTP Requests

Requests should use the newer version of the HTTP protocol: **HTTP/2**

The difference between HTTP/1 and HTTP/2 it that in **HTTP/1** the static assets HTML, CSS, JS, images, can only be download by the browser **one at a time**

To get around this limitations, browsers opened multiple connections at once, usually to a maximum around 6.

So to requests can be fired at the same time but for each of these connections, there must be:

- DNS Lookup
- TCP Handshake
- TLS Handshake

**In HTTP/2 Connection**:

Browser sends multiple requests in parallel over a single connection.

HTTP/3 also is supported my modern browsers

To disable HTTP/2 in WebPageTest click on Chromium and in command line input add:

```
--disable-http2 --disable-quic
```

The second disables HTTP/3 requests

---

## 3rd Party Static Assets

Developers include static assets from 3rd parties because it's quic, it's convenient and usually are served via a cdn. So they will be loaded faster no matter where the user is browsing from.

However depending on somebody else to deliver key static resources to your website is not without risk.

**Risks of 3rd Party Static Assets**

- Security
- 3rd party downtime, and CSS is render-blocking
- Network negotiation
- Loss of priority

Copy the content of third parties to the www directory and update each script's `src` attribute to point to the new file.

**Comparison**

The website's rending speed is improved, which can be seen in a video comparison.

Time to interactive metric has been reduced by nearly a quarter. This is because like with CSS, we downloading from the same server. In addition one of the prerequisites is that page content is visible.

---

## Image Resizing

PNG format isn't very efficient as these formats are typically more suited to icons, illustrations and other similar image types.

Should change to JPEG

We can reduce the file size of the website by trimming down dimensions of images.

**Resize**

Appropriate image size depends on the device. Should create to variants of this JPEG, one for desktop and one for mobile.

- 1110 px wide for desktop, the size of the images container.

- 375p px wide for mobile, roughly between mobile and tablet viewport sizes.

**Implementation**

Use `picture` element to allow more granular control on when we can switch an image to it's smaller mobile variant and vice versa.

```
<picture>
  <source srcset="img/slide1-sm1.jpg" type="image/jpg" media="(max-width: 768px)">
  <img src="img/slide1.jpg" alt="MLE Power wind turbine at work" >
</picture>
```

**Comparison**

Improvement to speed index and Largest Contentful Paint. The View Port content, which these metrics measure is also completing much faster. Very good for the website's perceived performance.

---
