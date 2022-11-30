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

## Image Optimization

**General Optimization**

- Using ImageOptim in Mac we can remove unnecessary data, such as metadata, thumbnails and core profiles which aren't needed on web

Can automate that process with **Gulp**

In `gulpfile.esm.js` root file:

```
function clean(){
  return glob('./www/*.{html,jpg,svg, webp}, {}, function(er, files ){
    for(let file in files){
      rimraf(files[file], () => {});
    }
  })
}
```

`npm install --save-dev gulp-imagemin

```
function compressImages(){
  return gulp.src('src/img/**/*')
  .pipe(imagemin([
    imagemin.mozjpeg({quality: 75, progressive: true})
  ]))
  .pipe(gulp.dist('www/img'))
}
```

```
function createWebP(){
  return gulp.src('www/img/*.jpg')
  .pipe(imagemin[
    imageinWebp()
  ])
  .pipe(rename(function(path){
    path.extname = '.webp';
  }))
  .pipe(gulp.dest('www/img))
}
```

```
const build = gulp.series(clean, compileNumjucks, compressImages, createWebP)
```

```
gulp build
```

**Progressive JPEGs**

Turning a passive wait into an active one.

**WebP Format**

This grabs all compressed JPEGs from the root `www` directory, creates a new WebP version for each of them and then uses the same filename with a different extension before saving them to the same directory.

We should keep the JPEG originals because not all browsers support WebPs. Because for browsers like Safari we need to server the original JPEGs.

Also WebPs don't support progressive loading.

**Server level config**

To serve the JPEG or WebP

---

**AnimatedGIFs**

Can be huge in file size. Can convert them to video in which for the modern web are much more optimized with a much lower file size.

Can be done using **FFmpeg**

```
ffmpeg -i www/img/dog.gif -b:v 0 -crf 25 -f mp4 -vcodec libx264 -pix_fmt yuv420p www/img/dog.mp4
```

Overall there is a serving of around 95% in file size between the GIF and video formats, which will do wonders for load times.

Like WebP, there is also WebM video format.

Replace the GIFs with video elements.

```
<video class="float-right ml-3 mb-3" autoplay loop muted playinline>
  <source src="/img/dog2.webp" type="video/webm">
  <source src="/img/dog2.mp4" type="video/mp4">
</video>
```

Overall downloads drop significantly

**Comparison**

Time to interactive is smaller by 15%

---

## Resource Hints

**Resource hints** are a way we can improve performance by telling the browser is needs to make connections to other domains or download content before it normally would.

Giving the browser a hot tip of what's to come.

You create resource hints in the head of HTML document using a link element like:

```
<link rel="preconnect" href="https://www.domain.com">
```

**DNS Prefetch**

Perform a DNS lookup on a domain

```
<link rel="dns-prefetch" href="https://www.3rd-party.com/">
```

**Preconnect**

Like DNS prefetch but the browser the goes on to make the TCP handshake and optional TLS handshake.

```
<link rel="preconnect" href="https://www.3rd-party.com">
```

Allows the browser to ready a full connection early.

**Prefetch**

We can tell the browser using a prefetch resource hint that it should download and cache the file in preparation - CSS JS HTML.

```
<link rel="prefetch" href="https://www.3rd-party.com/3rd-party.js" as="script">
```

Not supported by Safari

Browser has determines if it has more important resources to load

**Preload**

Very similar to prefetch except that is no longer a suggestion, it's an order.

```
<link rel="preload" href="https://www.3rd-party.com/3rd-party.js" as="script">
```

Firefox doesn't support it

**Prerender**

The most extensive resource hint.

In addition to opening a connection and downloading the pages HTML, the browser goes on to parse it's content and then download and execute it's discovered resources as well.

```
<link rel="prerender" href="https://example.com/other-page">
```

Prerender is best used where you can predict the user's next move with the high level of accuracy, like results page following a search or a multi step checkout journey.

Currently only supported in Chrome.

Preload is better suited to loading a page's own critical resources

---

## Async / Defer JavaScript

HTML Parsing is paused when the browser finds a script element until this request is completed and the script is executed.

This makes JavaScript a blocking resource as it delays rendering.

**DEFER**

Delays JavaScript execution until after the HTML has been fully parsed.

```
<script src="script.js" defer></script>
```

The HTML parser pause is no longer required, meaning the script is no longer render blocking.

**ASYNC**

Very similar except that JavaScript execution happens as soon is able to, parallel to the HTML parsing.

```
<script src="script.js" async></script>
```

Best suited for JavaScript which doesn't require a complete DOM or a dependency of other scripts

---

## Text Compression

When text assets like HTML CSS and JS are sent from the server to the browser you can compress it with **gzip**

The other widely used format is **BROTLI** which was developed by Google and allows you to achieve even greater levels of compression.

Typically the larger the file, the higher the compression level.

Both gzip and BROTLI are supported by Apache and NGINX.

In WebPageTest can disable compression with:

custom headers:

```
accept-encoding: randomstring
```

Going from compressed with gzip and BROTLI to uncompressed means 275% more bytes being transfered.

---

## Text Asset Optimization

By bundling the CSS and JS into single files will help the browser to download them faster.

This is because compression formats like GZIP and BROTLI, work better on larger files.

**CSS**

Bundle CSS into a single file using Sass.

Because Sass is a pre-processor language, it needs to be compiled to CSS.

Can install the Sass dependencies with gulp

**JavaScript**

In the global .js file, include the other scripts on the top.

Can use rollup to bundle files.

---

## Critical CSS

If we look at the waterfall view, CSS is now the only remaining render blocking resource preventing the critical rendering path from completing immediately.

This is CSS default behavior as if page render wasn't blocking until CSS is downloaded and parsed, the page would render before the browser knows how to style it and you would experienced what's called flash un-styled content.

However, there is a way to stop CSS blocking page render AND sidestep the flash un-styled content issue.

This is done by determining the bare minimum CSS required to style the content in initial viewport, as that all users sees as the page is loading. We then **separate this critical CSS from the rest** and **display it inline** in HTML document directly.

This removes CSS from the critical rendering path entirely, solving the render blocking problem.

How do you separate critical CSS from non critical, and make the latter load asynchronously?

```
npm i critical
```

import it into the `gulp` and use the task:

```
function generateCriticalCSS(){
  return glob('./www/*.html', {}, function(er, files){
    for(let file in files){
      const filename = path.basename(files(file));
      critical.generate({
        inline: true,
        base: 'www/',
        targe: filename,
        width: 1300,
        height: 900,
        ignore: {
          arule: ['@font-face']
        }
      })
    }
  }
}
```

We can use the preload resource hint to force the browser load this critical images sooner - logo and hamburger icon.

That's how we should be able to improve our websites perceived performance even if it doesn't impact the actual performance at all.

we can add the media attribute so the browser knows how to preload each.

```
<link rel="preload" href="/img/slide1-sm1.jpg" as="image" media="(max-width: 768px)">
<link rel="preload" href="/img/slide1.jpg" as="image" media="(min-width: 769px)">
```

---

## Google Fonts Optimization

Google Fonts is the most popular way of including no standard fonts on the website out of the box. Performance is already quite good.

The request returns CSS font-face rules which the browser can the use to download WOFF2 files, which modern browsers is the format most commonly used for web fonts.

These are then applied to the pages text, provided the correct family CSS

Google Font files are what is called, late discovered resources.

We can at least preconnect to the domain, to get the network negotiation out of the way, as early as possible.

```
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

You need `crossorigin` to satisfy the font-face specification.

Another popular recommendation is to preload the initial CSS:

```
<link rel="preload" href="https://fonts.googleapis.com/...">
```

---

## Self-Hosted Fonts

An alternative to using Google Fonts, is hosting them ourselves.

Self-hosted fonts open the do to more optimizations, as we now we have control how these fonts are downloaded, during the loading process.

**Setup Self Hosted**

- Create a new directory in the root, called fonts

- Download Lato, grab the links from Google font CSS and download all files

- Copy the font-face CSS from google-webfonts-helper and copy it in the `style.scss`

- Download the files

```
@font-face {
  font-family: "Lato";
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: local('Lato italic'), local('Lato-Italic'),
      url('/fonts/lato-v17-latin-italic.woff2') format('woff2'),
      url('/fonts/lato-v17-latin-italic.woff') format('woff'),
}
```

the `local` function, all it does is tell the browser that if the user already has this font installed locally on their machine, don't waste time re-downloading it.

Flash of invisible text it's happening because of how browsers load fonts, which can be controlled by a CSS property, which is called `font-display`

The value `swap` tells the browser to display text using the next available fallback font, until the primary choice is fully downloaded.

The period where the fallback font is used is typically called the flash of unstyled text.

For Google fonts CSS the swap is already there.

**Preload Google Fonts**

```
<link rel="preload" href="/fonts/lato-v17-lato-700.woff" as="font" type"font/woff" crossorigin>
```

The `type` attribute is used to tell the browser, that if it doesn't support this format, it can ignore this preload. In that instance, it will then hit the second preload to the standard woff format, which it will support.

We can remove the web fonts preload if the performance it's not better

**System Fonts**

Already use in the device

Typically used by a font face like this:

```
html {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"
}
```

Bootstrap has system fonts by default

---

## Lazy Loading

Lazy loading allows to defer the loading of assets until they are in the viewport and visible to the user

This means we only download what we need.

**Lazy Loading Images**

In Chrome, Edge, FireFox just add the `lazy` attribute.

Native lazy loading isn't only supported by Chromium or FireFox

Cannot polyfill the functionality 100%, can install the polyfill module.

**Lazy Loading iframes**

Can use the same attribute as images `lazy`, can really minimize the amount of bytes downloaded during first load

Only Chromium browser support this

**Minimizing Content Shift**

We can add `width` and `height` attributes to the image.

So that the image can be properly sized before it's downloaded.

Also applies to video elements

---

## Remove Unused CSS

In **Chrome Dev Tools > Coverage** we can see the unused CSS.

**Update HTML**

Each page should use it's own CSS

**Duplicate CSS**

Duplicate `style.css` for each page.

**Filter Unused CSS**

Can automate this process with a module called `PurgeCSS`

Example in gulp:

```
function removeUnusedCSS(){
  return glob('./www/*.html', {}, function(er), files){
    for(let file in files){
      const filename = path.basename(files[file], '.html')
      gulp.src('./www/${filename}.css)
      .pipe(purgeCSS({
        content: [`./www/${filename}.html`]
      }))
      .pipe(gulp.dest('./www/'))
    }
  }
}
```

and add it to the build task.

You can add a safelist with regex patterns in purge CSS for e.g. to ignore carousel dynamic elements.

---
