# Performance Observer

> Generic interface for getting [PerformanceObserver](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserver)'s performance measurement events.

## Install

```
npm install @sumup/performance-observer --save
```

or

```
yarn add @sumup/performance-observer
```

## Usage

### Subscribe to individual events

#### First Paint

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('first-paint',
  ({ name, duration })) => {
    console.log(`"${name}": ${duration}ms;`);
    // e.g. "first-paint": 1040ms;
  }
);
```

#### First Contentful Paint

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('first-contentful-paint',
  ({ name, duration })) => {
    console.log(`"${name}": ${duration}ms;`);
    // e.g. "first-contentful-paint": 1041ms;
  }
);
```

#### First Input Delay

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('first-input-delay',
  ({ name, duration })) => {
    console.log(`"${name}": ${duration}ms;`);
    // e.g. "first-input-delay": 20ms;
  }
);
```

#### User Timing

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('user-timing',
  ({ name, duration })) => {
    console.log(`"${name}": ${duration}ms;`);
    // e.g. "my-task": 3022ms;
  }
);

// start recording the time immediately before running a task
window.performance.mark('my-task:start');
await runMyTask();

// stop recording the time immediately after running a task
window.performance.mark('my-task:end');
window.performance.measure(
  'my-task',
  'my-task:start',
  'my-task:end'
);
```

#### Element Timing

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('element-timing',
  ({ name, duration })) => {
    console.log(`"${name}": ${duration}ms;`);
    // e.g. "hero-image-paint": 120ms;
  }
);
```

```html
<img elementtiming="hero-image-paint" src="example.png" />
```

#### Resource Timing

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('resource-timing',
  ({ name, url, duration })) => {
    console.log(`"${name}" (${url}): ${duration}ms;`);
    // e.g. "resource-timing" (http://sumup.com/file.js): 248ms;
  }
);
```

#### Navigation Timing

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observe('navigation-timing',
  ({ name, url, duration })) => {
    console.log(`"${name}" (${url}): ${duration}ms;`);
    // e.g. "navigation-timing" (http://sumup.com/products): 1352ms;
  }
);
```

### Subscribe to all events in one batch

#### Custom set of events

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver([
    'first-contentful-paint',
    'resource-timing'
]);

performanceObserver.observeAll(({ name, url, duration }) => {
    if (url) {
        console.log(`"${name}" (${url}): ${duration}ms;`);
    } else {
        console.log(`"${name}": ${duration}ms;`);
    }
    // handler will be called 2 times with such outputs, e.g:
    // "first-contentful-paint": 1041ms;
    // "resource-timing" (http://sumup.com/image.png): 1272ms;
});
```

#### Default set of events (`first-paint`, `first-contentful-paint`, `first-input-delay`):

```js
import createPerformanceObserver from '@sumup/performance-observer';

const performanceObserver = createPerformanceObserver();

performanceObserver.observeAll(({ name, url, duration }) => {
    console.log(`"${name}": ${duration}ms;`);
    // handler will be called 3 times with such outputs, e.g:
    // "first-paint": 1041ms
    // "first-contentful-paint": 1042ms
    // "first-input-delay": 20ms
});
```

### Accessing raw [PerformanceEntry](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry)

### Disconnecting observers

## API

### Methods

#### `observe(metricName: string, handlerFn)`

#### `observeAll(handlerFn)`

#### `disconnectAll()`

## Supported events

-   first-paint ("paint" entry)
    -   https://developer.mozilla.org/en-US/docs/Web/API/PerformancePaintTiming
-   first-contentful-paint ("paint" entry)
    -   https://developer.mozilla.org/en-US/docs/Web/API/PerformancePaintTiming
    -   https://web.dev/fcp
-   first-input-delay ("first-input" entry)
    -   https://developer.mozilla.org/en-US/docs/Glossary/First_input_delay
    -   https://web.dev/fid
-   user-timing ("measure" entry)
    -   https://developer.mozilla.org/en-US/docs/Web/API/PerformanceMeasure
    -   https://web.dev/custom-metrics/#user-timing-api
-   element-timing ("element" entry)
    -   https://web.dev/custom-metrics/#element-timing-api
-   resource-timing ("resource" entry)
    -   https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming
    -   https://web.dev/custom-metrics/#resource-timing-api
-   navigation-timing ("navigation" entry)
    -   https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming
    -   https://web.dev/custom-metrics/#navigation-timing-api

## Maintainers

-   [Dmitri Voronianski](mailto:dmitri.voronianskyi@sumup.com)

## About SumUp

![SumUp logo](https://raw.githubusercontent.com/sumup-oss/assets/master/sumup-logo.svg?sanitize=true)

It is our mission to make easy and fast card payments a reality across the _entire_ world. You can pay with SumUp in more than 30 countries, already. Our engineers work in Berlin, Cologne, Sofia and Sāo Paulo. They write code in JavaScript, Swift, Ruby, Go, Java, Erlang, Elixir and more. Want to come work with us? [Head to our careers page](https://sumup.com/careers) to find out more.
