# Front-End Performance Best Practices

This is a guideline of best practices that we can apply to our front-end project.
This guide will explore the causes of front-end performance issues and provide a list of best practices for optimizing web apps.
These tips are based on books, articles and professional experience.

## Table of Contents

1. [Follow the PRPL pattern](#follow-the-prpl-pattern)
2. [Follow the RAIL model](#follow-the-rail-model)
3. [Code minification](#code-minification)
4. [Bundle compression](#bundle-compression)
5. [Remove template whitespace](#remove-template-whitespace)
6. [Trim the HTML](#trim-the-html)
7. [Tree-shaking](#tree-shaking)
8. [Organize and remove unused imports](#organize-and-remove-unused-imports)
9. [Code-splitting](#code-splitting)
10. [Evaluate frameworks and dependencies](#evaluate-frameworks-and-dependencies)
11. [Unnecessary use of third-party packages](#unnecessary-use-of-third-party-packages)
12. [Lazy-Loading resources](#lazy-loading-resources)
13. [Do not lazy-load default route](#do-not-lazy-load-default-route)
14. [Lazy-load the images on a page](#lazy-load-the-images-on-a-page)
15. [Use Intersection Observer](#use-intersection-observer)
16. [Pre-fetching resources](#pre-fetching-resources)
17. [Caching](#caching)
18. [Prefer promises over callbacks](#prefer-promises-over-callbacks)
19. [Raw JavaScript can be faster than a library](#raw-javascript-can-be-faster-than-a-library)
20. [Place scripts at the bottom of the page](#place-scripts-at-the-bottom-of-the-page)
21. [Use application shell](#use-application-shell)
22. [Use service workers](#use-service-workers)
23. [Use web workers](#use-web-workers)
24. [Use server-side rendering](#use-server-side-rendering)
25. [Use partial hydration](#use-partial-hydration)
26. [Avoid content reflow](#avoid-content-reflow)
27. [Optimize images, videos, and fonts](#optimize-images-videos-and-fonts)
28. [Extract critical CSS](#extract-critical-css)
29. [Remove unused CSS](#remove-unused-css)
30. [Remove unused fonts](#remove-unused-fonts)
31. [Use system fonts](#use-system-fonts)
32. [Optimize rendering performance](#optimize-rendering-performance)
33. [Reduce HTTP requests](#reduce-http-requests)
34. [Support HTTP/2](#support-http2)
35. [Simulate 3G and older CPUs](#simulate-3g-and-older-cpus)
36. [Use cache-control header](#use-cache-control-header)
37. [Avoid inefficient iterations](#avoid-inefficient-iterations)
38. [Avoid console.log()](#avoid-consolelog)
39. [Avoid poor event handling](#avoid-poor-event-handling)
40. [Design for immutability](#design-for-immutability)
41. [Use Optimistic UI](#use-optimistic-ui)
42. [Use animations for better perception of performance](#use-animations-for-better-perception-of-performance)
43. [Scope Hoisting](#scope-hoisting)
44. [Async & Defer](#async--defer)
45. [Keep DOM access to a minimum](#keep-dom-access-to-a-minimum)
46. [Use virtual scrolling](#use-virtual-scrolling)
47. [Throttle and debounce](#throttle-and-debounce)
48. [Establish a performance culture](#establish-a-performance-culture)
49. [Choose the right metrics](#choose-the-right-metrics)
50. [Continuous Monitoring](#continuous-monitoring)

## Follow the PRPL pattern

[PRPL](https://web.dev/apply-instant-loading-with-prpl/) describes a pattern used to make web pages load and become interactive, faster:

- Push (or preload) the most important resources.
- Render the initial route as soon as possible.
- Pre-cache remaining assets.
- Lazy load other routes and non-critical assets.

It is meant to serve to a client (CSS, JS and other static assets) only to what will be used by the client on a current requested page. Upon that, resource will be cached.

## Follow the RAIL model

[RAIL](https://web.dev/rail/) is a user-centric performance model that provides a structure for thinking about performance.
RAIL stands for four distinct aspects of web app life cycle:

- Response (feedback in less than 100ms)
- Animation (60fps = 16ms per frame)
- Idle (intermediate state, 50ms blocks)
- Load (FMP as fast as possible)

## Code minification

[Minification](https://developers.google.com/speed/docs/insights/MinifyResources) refers to the process of removing unnecessary or redundant data without affecting how the resource is processed by the browser.
Bundling the application's components into `*.js`, `*.css` and `*.html` files and passing them through a JavaScript minification program will make the code leaner, reducing its size and thus make it load faster.
Tools like [webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/) or [Parcel](https://parceljs.org/) have support to code minification.

## Bundle compression

[Compression](https://en.wikipedia.org/wiki/HTTP_compression) is used to reduce file size by using a compression scheme such as [Gzip](https://pt.wikipedia.org/wiki/Gzip) or [Brotli](https://en.wikipedia.org/wiki/Brotli).
Compression of the responses' payload standard practice for bandwidth usage reduction.
By specifying the value of the header Accept-Encoding, the browser hints the server which compression algorithms are available on the client's machine.
On the other hand, the server sets the value for the Content-Encoding header of the response in order to tell the browser which algorithm has been chosen for compressing the response.

## Remove template whitespace

Although we don't see the whitespace character (a character matching the \s regex) it is still represented by bytes which are transferred over the network.
If we reduce the whitespace from our templates (html files) to the minimum we will be respectively able to drop the bundle size.
We can use tools like [webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/) or [Parcel](https://parceljs.org/) for this job.

## Trim the HTML

A large DOM tree can slow down the page performance in multiple ways such as network efficiency and load performance, runtime performance or memory performance.
The complexity of the HTML plays a large role in determining how long it takes to query and modify DOM objects.
If we can cut our application's HTML by half, we could potentially double the DOM speed.
That's a tough goal, but we can start by eliminating unnecessary `<div>` and `<span>` tags.
[Google Lighthouse](https://developers.google.com/web/tools/lighthouse) flags pages with DOM trees that have more than 1500 nodes total, have a depth greater than 32 nodes, and have a parent node with more than 60 child nodes.

## Tree-shaking

[Tree-shaking](https://webpack.js.org/guides/tree-shaking/) or dead code elimination is a technique that is applied when optimizing code written in ECMAScript dialects like JavaScript or TypeScript into a single bundle that is loaded by a web browser.
Often contrasted with traditional single-library dead code elimination techniques common to minifiers, tree shaking eliminates unused functions from across the bundle by starting at the entry point and only including functions that may be executed.
Tools like [webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/) or [Parcel](https://parceljs.org/) have support to tree-shaking.

## Organize and remove unused imports

With clean and easy to read import statements we can quickly see the dependencies of current code.
With [no-unused-variable](https://palantir.github.io/tslint/rules/no-unused-variable/) lint rule are automatically remove unused imports, variables, functions, and private class members, when using TSLint's `--fix` option.
[ordered-imports](https://palantir.github.io/tslint/rules/ordered-imports/) lint rule requires that import statements be alphabetized and grouped.
Equivalent rules for ordering imports and unused variables and imports are available for [ESLint](https://eslint.org/).

## Code-splitting

[Code-splitting](https://developer.mozilla.org/en-US/docs/Glossary/Code_splitting) is another technique that splits the codebase into "chunks" that are loaded on demand.
As an application grows in complexity or is maintained, CSS and JavaScript files or bundles grow in byte size.
The old "golden rule" was to minify everything into a single JS file, but this is far from ideal.
Send what the user needs only when the user needs it.
Code splitting is a feature supported by bundlers like [webpack](https://webpack.js.org/) and [Browserify](https://browserify.org/) which can create multiple bundles that can be dynamically loaded at runtime.
This is done through [Dynamic Importing](https://javascript.info/modules-dynamic-imports).

## Evaluate frameworks and dependencies

Not every project needs a framework and not every page of a single-page-application needs to load a framework.
Once a framework is chosen, we'll be staying with it for at least a few years, so if we need to use one, we need make sure our choice is informed and well considered and that goes especially for key performance metrics that we care about.
We could go as far as evaluating the candidates on Sacha Greif's [12-point scale scoring system](https://www.freecodecamp.org/news/the-12-things-you-need-to-consider-when-evaluating-any-new-javascript-library-3908c4ed3f49/).

## Unnecessary use of third-party packages

Review the third-party packages we are using and see if better and smaller alternative is available as it may reduce the final size of the bundle.
If we include a third-party package just to achieve a small functionality which could be easily done natively with JavaScript or Framework, then we are adding unnecessary size overhead to the app which could have been easily saved.
Tools such as [Bundlephobia](https://bundlephobia.com/) and [webpack-bundle-analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer) can be used to check the bundle size of each package.

## Lazy-Loading resources

[Lazy-loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading) is the mechanism where instead of loading complete app, we load only the modules which are required at the moment thereby reducing the initial load time.
In simple words, it means "don't load something which you don't need."
This practice essentially involves splitting the code at logical breakpoints, and then loading it once the user has done something that requires, or will require, a new block of code.
This speed up the initial load of the application and lightens its overall weight as some blocks may never even be loaded.

## Do not lazy-load default route

Let's suppose we have the following [Angular Routing](https://angular.io/guide/router) configuration:

```typescript
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', loadChildren: () => import('./dashboard.module').then(mod => mod.DashboardModule) }
];
```

The first time the user opens the application using the url: `https://example.com/` they will be redirected to `/dashboard` which will trigger the lazy-route with path dashboard.
In order Angular to render the bootstrap component of the module, it will has to download the file `dashboard.module` and all of its dependencies.
Triggering extra HTTP requests and performing unnecessary computations during the initial page load is a bad practice since it slows down the initial page rendering.

## Lazy-load the images on a page

[Lazy loading images](https://css-tricks.com/the-complete-guide-to-lazy-loading-images/) is a technique that defers the loading of images on a page to a later point in time, instead of loading them up front.
The most common lazy-loading candidates are images as used in `<img>` elements.
Several techniques are available such as [browser-level lazy-loading](https://web.dev/lazy-loading-images/#images-inline-browser-level), [intersection observer](https://web.dev/lazy-loading-images/#images-inline-intersection-observer) or [using scroll and resize event handlers](https://web.dev/lazy-loading-images/#images-inline-event-handlers).
Be aware that lazy loading might reduce the initial page load, but it also might result in a bad user experience if some images are deferred when they should not be.

## Use Intersection Observer

In general, it's recommended to lazy-load all expensive components, such as heavy JavaScript, videos, iframes, widgets, and potentially images.
The most performant way to do slightly more sophisticated lazy loading is by using the [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) that provides a way to asynchronously observe changes in the intersection of a target element with an ancestor element or with a top-level document's viewport.
[Native lazy-loading](https://web.dev/browser-level-image-lazy-loading/) is available for images and iframes with the loading attribute.

## Pre-fetching resources

Technically speaking, [resource hints](https://w3c.github.io/resource-hints/) are different values for the `rel` attribute of the `<link>` HTML element used for external resources.
Use resource hints to save time on `dns-prefetch`, `preconnect`, `prefetch` and `preload`.
These primitives enable the developer, and the server generating or delivering the resources, to assist the user agent in the decision process of which origins it should connect to, and which resources it should fetch and pre-process to improve page performance.

## Caching

Caching is another common practice intending to speed-up our application by taking advantage of the heuristic that if one resource was recently been requested, it might be requested again in the near future.
For caching data, we usually use a custom caching mechanism.
For caching static assets in the web browser, we can either use the standard browser caching or Service Workers with the [CacheStorage API](https://developer.mozilla.org/en-US/docs/Web/API/Cache).
On the server, there are different ways, like using a [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) or a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy), are commonly used.

## Prefer promises over callbacks

Promises are easy to use and anything with a callback can be "promisified".
Callbacks are synchronous and with promises and `async...await`, we get to do things asynchronous which help speed up the code, especially because JavaScript is single-threaded.

## Raw JavaScript can be faster than a library

JavaScript libraries, such as [jQuery](https://jquery.com/), can save us an enormous amount of time when coding, especially with AJAX operations.
Having said that, always keep in mind that a library can never be as fast as raw JavaScript.
jQuery's `each` method is great for looping, but using a native `for` statement will always be an ounce quicker.

## Place scripts at the bottom of the page

When loading a script, the browser can't continue until the entire file has been loaded.
Thus, the user will have to wait longer before noticing any progress.
If we have JavaScript files whose only purpose is to add functionality, for example, after a button is clicked, we place those files at the bottom, just before the closing body tag.
The primary goal is to make the page load as quickly as possible for the user.
This is absolutely a best practice.

## Use application shell

An [application shell](https://developers.google.com/web/updates/2015/11/app-shell) is the minimal HTML, CSS, and JavaScript powering a user interface that we show to the users in order to indicate them that the application will be delivered soon.
An application shell is the secret to reliably good performance.
Frameworks like [Angular have app shell support](https://angular.io/guide/app-shell).

## Use service workers

We can think of the [Service Worker](https://developers.google.com/web/fundamentals/primers/service-workers) as an HTTP proxy which is located in the browser.
All requests sent from the client are first intercepted by the Service Worker which can either handle them or pass them through the network.
If the website is running over HTTPS, we can cache static assets in a service worker cache and store offline fallbacks and retrieve them from the user's machine, rather than going to the network.
There are a number of use cases for a Service Worker.
For example, we could implement "Save for offline" feature, handle broken images, introduce messaging between tabs or provide different caching strategies based on request types.

## Use web workers

Use [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) when we need to execute code that needs a lot of execution time.
Web Workers help us to run scripts in background threads.
The worker thread can perform tasks without interfering with the user interface.
They can perform processor-intensive calculations without blocking the user interface thread.

## Use server-side rendering

A big issue of the traditional SPA is that they cannot be rendered until the entire JavaScript required for their initial rendering is available.
[Server-side rendering](https://developers.google.com/web/updates/2019/02/rendering-on-the-web) solves this issue by pre-rendering the requested page on the server and providing the markup of the rendered page during the initial page load.
Combining client-side rendering with server-side rendering improves application performance.
The use of both techniques is recommended.
[Angular](https://angular.io/guide/universal), [React](https://reactjs.org/docs/react-dom-server.html) and [Vue](https://ssr.vuejs.org/) have server-side rendering support.

## Use partial hydration

Hydration is the process by which pre-rendered content is made interactable.
With the amount of JavaScript used in applications, we need to figure out ways to send as little as possible to the client.
Particularly static web sites, we might notice there are parts of the site that really are just visual and don't change.
Instead of doing SSR and then sending the entire app to the client, only small pieces of the app's JavaScript would be sent to the client and then hydrated.
For example, you can read [partial hydration with Next and Preact](https://medium.com/@luke_schmuke/how-we-achieved-the-best-web-performance-with-partial-hydration-20fab9c808d5) article.

## Avoid content reflow

[Preventing content reflow](https://css-tricks.com/preventing-content-reflow-from-lazy-loaded-images/) is another trivial point, which if solved, can help maintain a good user experience.
When there is no image or content, the browser doesn't know the size it will take up. If we do not specify it using CSS, then the enclosing container would have no dimensions.
This can be avoided by specifying a height and/or width for the enclosing container so that the browser can paint the container with a known height and width.

## Optimize images, videos, and fonts

When we're working on a landing page on which it's critical that a hero image loads blazingly fast, make sure that JPEGs are progressive and compressed.
So, at the very least, we could explore using the [WebP](https://en.wikipedia.org/wiki/WebP) format for our images.
[AVIF](https://caniuse.com/avif) is a new image format derived from the keyframes of [AV1](https://caniuse.com/av1) video.
Compared to WebP and JPEG, AVIF performs significantly better, yielding median file size savings for up to 50%.
For PNG, we can use [Pingo](https://css-ig.net/pingo.php), and for SVG, we can use [SVGO](https://www.npmjs.com/package/svgo).
Use [BlurHash](https://blurha.sh/) if you'd like to show a placeholder image early.
Unlike with images, browsers do not preload `<video>` content, but HTML5 videos tend to be much lighter and smaller than GIFs.
[AV1](http://aomedia.org/av1/) has good chances of becoming the ultimate standard for video on the web.
There are many options for web font loading, and we can choose one of the strategies from [Comprehensive Guide to Font-Loading Strategies](https://www.zachleat.com/web/comprehensive-webfonts/).
[WOFF2](https://caniuse.com/woff2) support is great, and we can use [WOFF](https://caniuse.com/woff2) as fallback for browsers that don't support it, or perhaps legacy browsers could be served system fonts.

## Extract critical CSS

When the browser renders a page, it has to wait for the CSS resources to be downloaded and parsed.
To ensure that browsers start rendering the page as quickly as possible, it's become a common practice to collect all of the CSS required to start rendering the first visible portion of the page (known as "[critical CSS](https://web.dev/extract-critical-css/)" or "above-the-fold CSS") and include it inline in the `<head>` of the page, thus reducing roundtrips.
We can then inline critical CSS and lazy-load the rest.

## Remove unused CSS

CSS files can easily gain redundant KBs over time.
Unused CSS just adds dead weight to our applications and contributes to the growth of web page size, so we want to make sure that we have as little excess code as possible.
Aside from slowing down our website's overall performance, excess CSS can cause headaches for developers.
Clean and orderly stylesheets are easier to maintain than disorderly ones.
We can [remove unused CSS](https://www.keycdn.com/blog/remove-unused-css) manually or with tools.
The most popular tools are [PurifyCSS](https://www.npmjs.com/package/purify-css), [PurgeCSS](https://www.npmjs.com/package/purgecss) and [UnCSS](https://www.npmjs.com/package/uncss).

## Remove unused fonts

Like JavaScript and CSS, it's a good idea to remove the unused fonts, which can help save few bytes over the network.
The disadvantages of web fonts, such as system fonts, are that they add extra HTTP requests to external resources.
Web fonts are also render blocking.

## Use system fonts

Sometimes the best font-loading strategy is not loading any font, and instead relying on the so-called [System Font Stack](https://systemfontstack.com/).
Defaulting to the system font of a particular operating system can boost performance because the browser doesn't have to download any font files, it's using one it already had.

## Optimize rendering performance

The browsers are super-fast, however, on complex websites, there are some painting issues.
We need to make sure that there is no lag when scrolling the page or when an element is animated.
CSS proprieties such as `box-shadow`, `border-radius`, `position`, `filter`, and even `width` and `height`, especially for complex animations or repetitive changes, require the browser to do complex re-calculations and repaint the view again down to every nested child.
The `will-change` is used as a performance boost to tell the browser about how a property is expected to change but it is used as a last resort.

## Reduce HTTP requests

When the browser fetches data from a server it does so using HTTP.
Every browser limits the [number of concurrent connections](https://www.linkedin.com/pulse/why-does-your-browser-limit-number-concurrent-ishwar-rimal) to a single domain as well as it has a limit for overall concurrent connections.
When there are more files and data needing to load than connections available, it is inevitable that blocking will occur.
The more HTTP requests the web page makes the slower it will load.
Using inline JavaScript/CSS, sprites and reducing the number of chunks it will improve the performance.

## Support HTTP/2

[HTTP/2](https://en.wikipedia.org/wiki/HTTP/2) protocol provides some great enhancements that will not only help improve the app performance but will also help speed up the site in general.
HTTP/2 uses multiplexing, therefore allowing multiple requests and responses to be sent at the same time.
With Google pushing towards a more secure HTTPS web over the last few years, a switch to HTTP/2 environment is definitely a good investment.
To enable HTTP/2 all that is required is an SSL certificate (it requires TLS) and a server that supports HTTP/2.

## Simulate 3G and older CPUs

We have two major constraints that effectively shape a reasonable target for speedy delivery of the content on the web.
On the one hand, we have network delivery constraints.
On the other hand, we have hardware constraints on memory and CPU due to JavaScript parsing and execution times.
We can simulate 3G and also throttle the CPU to test the site as if it was running on a slower or older device.
We can use the [Chrome DevTools Performance](https://developer.chrome.com/docs/devtools/evaluate-performance/) panel to analyse runtime performance.

## Use cache-control header

The [cache-control header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) holds directives for caching in both requests and responses.
Cache-control header controls who caches the response under what condition and for how long thus eliminating the need for network round trip for the resources which are cached.
A typical example where we want to use Cache-Control are CSS/JavaScript assets with a hash in their name.
For them, we probably want to cache as long as possible, and ensure they never get re-validated:

```bash
Cache-Control: public, max-age: 31556952
```

## Avoid inefficient iterations

Removing unnecessary loops or calls within loops will speed up your JavaScript performance.
The `for` loop is the fastest way.
Caching the length makes the loop performs better.
The `forEach` is slower than the `for` loop, so it's probably better to avoid it, especially for large arrays.
However, unless we are desperate for performance at the code level (which is rare), make it readable.
For example, we can use the `for` loop in server-side applications and the array methods in client-side applications.

## Avoid console.log()

Using `console.log()` statements in production code could be a bad idea as it will slow down the performance of the app and also logging objects with `console.log()` creates memory leak issue.
When browser's console window is open, the `console.log()` execution slows down even further by many times thus impacting site's performance significantly.
It's better to completely remove the `console.log()` statements from production code or at least have an environment specific conditional logging.

## Avoid poor event handling

Adding event listeners to the DOM node could create memory leak issue.
If we forget to remove the listener, it will hold a reference to a DOM node even if it is removed from the document.
Proper use of event handlers can improve performance by reducing the depth of the call stack.
Scroll performance can be improved with [passive event listeners](https://developers.google.com/web/updates/2016/06/passive-event-listeners) by setting a flag in the options parameter.
So, browsers can scroll the page immediately, rather than after the listener has finished.

## Design for immutability

The immutability in JavaScript allows us to differentiate objects and track changes in our objects.
It may sound petty and insignificant, but it turns out to be crucial, especially in front-end applications.
In frameworks such as `Angular`, `React` and `Vue`, we'll actually get a performance boost by using immutable data structures.
If we are using [Redux](https://redux.js.org/) for state management, then we would naturally get a new instance every time the state changes, which will trigger change detection for components when provided to a component's inputs.
We gain predictability, change tracking, easiness of implementing reactive interface, change history and others such as testability and a single source of truth.

## Use Optimistic UI

We know when we "like" a tweet, the visual response appears immediately even before the API is reached.
This concept is called [Optimistic UI](https://medium.com/distant-horizons/using-optimistic-ui-to-delight-your-users-ac819a81d59a).
This is a very simple solution to avoid some part of the loading states during the updates and pretending that some long-time actions are called immediately.
Work as if everything would work, eliminating intermediate states.

## Use animations for better perception of performance

Animations are a great resource to buy time while something is loading. This concept is called "Active Wait".
While loading assets, we can try to always be one step ahead of the customer, so the experience feels swift while there is quite a lot happening in the background.
To keep the customer engaged, we can test skeleton screens instead of loading indicators, add transitions/animations and basically cheat the UX when there is nothing more to optimize.

## Scope Hoisting

Scope determines the accessibility of variables inside a JavaScript program.
Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.
While modules make their way to be natively supported, bundlers like [webpack](https://webpack.js.org/) transform our import and export statements into valid JavaScript code that can run in browsers today.
Using Scope Hoisting when bundling the app allows smaller builds and ESM output.

## Async & Defer

These two attributes are a must for increasing speed and performance of websites.
They allow the elimination of render-blocking JavaScript where the page would have to load and execute scripts before finishing to render the page.
With `async` the page load is interrupted while the asset is executed, and with `defer` the execution is postponed for after the page load.

## Keep DOM access to a minimum

Accessing the DOM in browsers is an expensive thing to do.
The DOM is a very complex API and rendering in browsers can take up a lot of time.
To make sure that our code is fast and doesn't slow down the browser to a halt try to keep DOM access to a bare minimum.
Instead of constantly creating and applying elements, have a tool function that turns a string into DOM elements and call this function at the end of our generation process to disturb the browser rendering once rather than continually.

## Use virtual scrolling

In [virtual scrolling](https://dev.to/adamklein/build-your-own-virtual-scroll-part-i-11ib), we don't display the entire content on the screen, to reduce the amount of DOM node rendering and calculations.
We "fool" the user to think the entire content is rendered by always rendering just the part inside the window, and a bit more on the top and bottom to ensure smooth transitions.
For Angular projects, we can use `<cdk-virtual-scroll-viewport>` directive from [Angular CDK](https://material.angular.io/cdk/scrolling/overview).

## Throttle and debounce

Setting limits on how much JavaScript gets executed at once can help fine tune the application's performance.
Throttling sets the maximum number of times that a function may be called over time, while debouncing ensures that a function isn't called again until a designated amount of time passes.
For example, we can throttling a button click so we can't spam click, throttling an API call, throttling a mousemove/touchmove event handler, debouncing a resize event handler, debouncing a scroll event handler, debouncing a save function in an autosave feature, etc.

## Establish a performance culture

In many organizations, front-end developers know exactly what common underlying problems are and what strategies should be used to fix them.
However, as long as there is no established endorsement of the performance culture, each decision will turn into a battlefield of departments, breaking up the organization into silos.
Without a strong alignment between dev/design and business/marketing teams, performance isn’t going to sustain long-term.
Only with buy-in from business stakeholders, and to get it, we need to establish and shown then a case study, or a proof of concept on how speed, especially [Core Web Vitals](https://web.dev/vitals/), benefits metrics and Key Performance Indicators (KPIs), which they care about.

## Choose the right metrics

Study what metrics matter most to the application.
Usually, the most specific and relevant ones are the Time to Interactive (TTI), First Input Delay (FID), Largest Contentful Paint (LCP), Total Blocking Time (TBT), Cumulative Layout Shift (CLS), Speed Index, CPU time spent, and others that make sense.
For more details, take a look at [Core Web Vitals](https://web.dev/vitals/).
Core Web Vitals are the subset of Web Vitals that apply to all web pages.
Each of the Core Web Vitals represents a distinct facet of the user experience, is measurable in the field, and reflects the real-world experience of a critical user-centric outcome.

## Continuous Monitoring

[Continuous Monitoring](https://en.wikipedia.org/wiki/Continuous_monitoring) is a process that organizations may implement to enable rapid detection of issues and risks.
Continuous Monitoring suggests monitoring all systems and infrastructure using several tools, dashboards, and alerts including real-time insights of different metrics.
Also, look into [Lighthouse](https://developers.google.com/web/tools/lighthouse), [Sitespeed](https://www.sitespeed.io/), [SpeedCurve](https://www.speedcurve.com/), [SpeedTracker](https://speedtracker.org/) and [Calibre](https://calibreapp.com/) to monitor changes in performance over time, which will give you a more detailed picture of the performance.

## Bibliography

- [5 Common that Affect Page Load Time](https://blog.bitsrc.io/5-common-mistakes-developers-do-that-affect-page-load-time-5a49b0e46f6b)
- [18 Tips for Website Performance Optimization](https://www.keycdn.com/blog/website-performance-optimization)
- [20 Best Practices for Improving JavaScript Performance](https://www.keycdn.com/blog/javascript-performance)
- [23 front-end performance engineering rules](https://techbeacon.com/app-dev-testing/23-front-end-performance-rules-web-applications)
- [35 quick tips to improve web performance](https://uxdesign.cc/35-quick-tips-about-web-performance-114b8fab0d6)
- [44 quick tips to fine-tune Angular performance](https://medium.com/faun/44-quick-tips-to-fine-tune-angular-performance-9f5768f5d945)
- [About PageSpeed Insights](https://developers.google.com/speed/docs/insights/v5/about)
- [Analyze runtime performance](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance)
- [Angular Performance Checklist](https://github.com/mgechev/angular-performance-checklist)
- [Angular Tools for High Performance](https://blog.angular.io/angular-tools-for-high-performance-6e10fb9a0f4a)
- [Every Performance tips for angular app](https://dev.to/imm9o/every-performance-tips-for-angular-app-25c4)
- [Front-End Performance Checklist 2021](https://www.smashingmagazine.com/2021/01/front-end-performance-2021-free-pdf-checklist/)
- [Improve Page Rendering Speed Using Only CSS](https://blog.bitsrc.io/improve-page-rendering-speed-using-only-css-a61667a16b2)
- [Spellbook of Modern Web Dev](https://github.com/dexteryy/spellbook-of-modern-webdev)
- [The Complete Guide To Angular Load Time Optimization](https://christianlydemann.com/the-complete-guide-to-angular-load-time-optimization/)
- [The Front-End Performance Checklist](https://codeburst.io/the-front-end-performance-checklist-speeds-up-your-web-developments-b68e1c7a0276)
