## Web Workers

Воркер — это скрипт, работающий в потоке, отдельном от потока браузера. Веб-воркеры являются воркерами общего типа. В отличие от сервис-воркеров и ворклетов, они не имеют конкретных вариантов использования, кроме возможности выполнения в отдельном потоке. Как результат веб-воркеры можно использовать для выгрузки почти любой тяжелой работы из основного потока, которая может долго выполняться или выполняться параллельно другим процессам. Хороший пример веб-приложение обработки изображений

const myWorker = new Worker('worker.js');

Как и все воркеры, веб-воркеры не имеют доступа к DOM, это значит, что любая необходимая информация должна быть передана между воркером и основным скриптом, используя window.postMessage()

/* main.js */

const myWorker = new Worker('worker.js'); // Create worker

myWorker.postMessage('Hello!'); // Send message to worker

myWorker.onmessage = function(e) { // Receive message from worker
  console.log(e.data);
}

В нашем скрипте воркера, мы можем слушать сообщения от основного скрипта и возвращать ответ.

/* worker.js */

self.onmessage = function(e) { // Receive message from main file
  console.log(e.data);
  self.postMessage(workerResult); // Send message to main file
}

## Service Workers

Особенности:
- выступает прокси между браузером и сетью и/или кэшем, позволяет контролировать запросы на сервер и их обрабатывать
- запускается в worker контексте, поэтому он не имеет доступа к DOM и работает в потоке, отдельном от основного потока JavaScript, а следовательно — не блокирует его. Он призван быть полностью асинхронным, поэтому использовать синхронные API (XHR и LocalStorage) в SW нельзя
- получают нужные данные из IndexedDB API
- работают только по HTTPS, так как давать посторонним людям возможность изменять сетевые запросы крайне опасно

/* main.js */

navigator.serviceWorker.register('/service-worker.js');

В отличие от обычных веб-воркеров, сервис-воркеры имеют дополнительные возможности, которые позволяют им выполнять свои функции прокси-сервера. После установки и активации сервис-воркеры могут перехватывать любые сетевые запросы из основного документа.

/* service-worker.js */

self.addEventListener('install', function(event) { // Install 
    // ...
});

self.addEventListener('activate', function(event) { // Activate 
    // ...
});

self.addEventListener('fetch', function(event) { // Listen for network requests from the main document
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        // Cache hit - return response
        if (response) {
          return response;
        }
        return fetch(event.request);
      }
    )
  );
});

После перехвата сервис-воркеры могут, например, ответить, вернув документ из кэша вместо того чтобы идти за ним в сеть, тем самым позволяя веб-приложению работать автономно.

self.addEventListener('fetch', function(event) {
    event.respondWith(
        caches.match(event.request); // Return data from cache
    );
});

## Worklets

Ворклеты — это хуки внутри rendering pipeline браузера, позволяющий нам иметь низкоуровневый доступ к процессу рендеринга браузера, таким как вычисление стилей и расчет макета.

## RAIL Model

RAIL is a user-centric performance model that provides a structure for thinking about performance. The model breaks down the user's experience into key actions and helps еto define performance goals for each of them. RAIL stands for four distinct aspects of web app life cycle: response, animation, idle, and load.

Response: process events in under 50ms

Goal:
- Complete a transition initiated by user input within 100 ms, so users feel like the interactions are instantaneous.

Guidelines:
- To ensure a visible response within 100 ms, process user input events within 50 ms. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
- Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100 ms window to do other expensive work, but be careful not to block the user. If possible, do work in the background.
- For actions that take longer than 50 ms to complete, always provide feedback.

Animation: produce a frame in 10 ms

Goals:
- Produce each frame in an animation in 10 ms or less. Technically, the maximum budget for each frame is 16 ms (1000 ms / 60 frames per second ≈ 16 ms), but browsers need about 6 ms to render each frame, hence the guideline of 10 ms per frame.
- Aim for visual smoothness. Users notice when frame rates vary.

Guidelines:
- In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100 ms response to pre-calculate expensive work so that you maximize your chances of hitting 60 fps.

Idle: maximize idle time

Goal:
- Maximize idle time to increase the odds that the page responds to user input within 50 ms.

Guidelines:
- Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
- Perform work during idle time in 50 ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50 ms.
- If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

Load: deliver content and become interactive in under 5 seconds

Goals:
- Optimize for fast loading performance relative to the device and network capabilities of your users. Currently, a good target for first loads is to load the page and be interactive in 5 seconds or less on mid-range mobile devices with slow 3G connections
- For next loads, a good target is to load the page in under 2 seconds.

Guidelines:
- Eliminate render blocking resources (for example  Coverage tab in Chrome DevTools)
- You don't have to load everything in under 5 seconds to produce the perception of a complete load. Consider lazy-loading images, code-splitting JavaScript bundles, and other optimizations
- Keep in mind that although your typical mobile user's device might claim that it's on a 2G, 3G, or 4G connection, in reality the effective connection speed is often significantly slower, due to packet loss and network variance.

Tools for measuring RAIL:
- Chrome DevTools 
- Lighthouse
- WebPageTest

## PRPL

PRPL is a pattern for structuring and serving web applications and Progressive Web Apps (PWAs) with an emphasis on improved app delivery and launch performance. The letters describe a set of ordered steps for fast, reliable, efficient loading:
- Push (preload) all resources required for the initial route – and only those resources – to ensure that they are available as early as possible
- Render the initial route and make it interactive before loading any additional resources (inline crytical JS and CSS, make other scripts as async, load other styles later or mark them for some cases)
- Pre-cache resources for additional routes that the user is likely to visit, maximizing responsiveness to subsequent requests and resilience under poor network conditions (Service workers)
- Lazy-load routes on demand as the user requests them; resources for key routes should load instantly from the cache, whereas less commonly used resources can be fetched from the network upon request

## JS Loading Optimization:
- Only sending the code a user needs (code-splitting, lazy-loading)
- Minification
- Compression
- Removing unused code (tree-shaking)
- Caching code to minimize network trips (HTTP caching, Service Workers

## First Contentful Paint (FCP):

This metric measures the time from when the page starts loading to when any part of the page's content is rendered on the screen. It can be only a part of content.

Tools: PageSpeed Insights, Lighthouse, Chrome DevTools.

How to improve FCP:
- Eliminate render-blocking resources
- Remove unused and minify CSS
- Preconnect to required origins
- Reduce server response times (TTFB)
- Avoid multiple page redirects
- Preload key requests

## Largest Contentful Paint (LCP):

This metric reports the render time of the largest image or text block visible within the viewport.

Tools: PageSpeed Insights, Chrome DevTools, Lighthouse, WebPageTest.

How to improve LCP:
- Apply instant loading with the PRPL pattern
- Optimizing the Critical Rendering Path
- Optimize your CSS, images, fonts and JS

## First Input Delay (FID):

FID measures the time from when a user first interacts with a page (i.e. when they click a link, tap on a button, or use a custom, JavaScript-powered control) to the time when the browser is actually able to begin processing event handlers in response to that interaction.

How to improve FID:
- Reduce the impact of third-party code
- Reduce JavaScript execution time
- Minimize main thread work
- Keep request counts low and transfer sizes small

## Time to Interactive (TTI):

The TTI metric measures the time from when the page starts loading to when its main sub-resources have loaded and it is capable of reliably responding to user input quickly.

How to improve TTI:
- Minify JavaScript
- Preconnect to required origins
- Preload key requests
- Reduce the impact of third-party code
- Minimize critical request depth
- Reduce JavaScript execution time
- Keep request counts low and transfer sizes small


