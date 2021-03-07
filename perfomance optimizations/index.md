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

## Worklets:

Ворклеты — это хуки внутри rendering pipeline браузера, позволяющий нам иметь низкоуровневый доступ к процессу рендеринга браузера, таким как вычисление стилей и расчет макета.
