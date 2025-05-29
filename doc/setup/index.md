# Setup: how to create this from scratch

This page describes how to create this entire application from scratch:

* Initlialize prequisties such as PNPM packages and TypeScript.

* Create files such as `index.html` and `document-load.ts`.

* Launch the WAuth app so it displays telemetry in the browser console.

## Create

Create demo using [PNPM](https://pnpm.io/) and [TypeScript](https://www.typescriptlang.org/):

```sh
mkdir wauth && cd $_
pnpm init
tsc --init
mkdir src
```

### git & .gitignore

Create file `.gitignore`:

```gitignore
node_modules
dist
.parcel-cache
```

Initialize git:

```sh
git init
git add --all
git commit -m "Init"
```

### Create files

Create these files in the directory `src`:

* [`index.html`](../../src/index.html) is a typical HTML page that shows the sign in dialog box, and also sends OpenTelemetry.

* [`index.css`](../../src/index.css) is a Cascading Style Sheet that makes the login prettier, such as with layout, fonts, etc.

* [`index.js`](../../src/index.js). This JavaScript handles HTML form input events such as onKeyDown, onKeyUp, onKeyPress, etc.

* [`document-load.ts`](../../src/document-load.ts). This TypeScript handles sending OpenTelemetry events to the console.

### Install OpenTelemetry packages

Install OpenTelemetry packages for the API, SDK, instrumentation, and context zone:

```sh
pnpm install @opentelemetry/api
pnpm install @opentelemetry/sdk-trace-base
pnpm install @opentelemetry/sdk-trace-web
pnpm install @opentelemetry/instrumentation-document-load
pnpm install @opentelemetry/context-zone
```

### Parcel

Install Parcel. This is a build toool that provides a development server, hot reloading, error diagnostics, and more.

Run:

```sh
pnpm install --save-dev parcel
```

Edit file `package.json` to change the build configuration.

Old:

```json
"main": "index.js",
```

New:

```json
"default": "index.html",
```

Build the app:

```sh
npx parcel src/index.html
```

You should see:

```stdout
Server running at http://localhost:1234
```

Browse <http://localhost:1234>.

You should see the HTML page with the login box.

### Console output

View your browser console output, such as via you browser's developer toolbar.

You should see lots of OpenTelemetry output that includes events like these:

```text
Object { name: "fetchStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "domainLookupStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "domainLookupEnd", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "connectStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "connectEnd", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "requestStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "responseStart", attributes: {}, droppedAttributesCount: 0, … }
​Object { name: "responseEnd", attributes: {}, droppedAttributesCount: 0, … }

Object { name: "unloadEventStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "unloadEventEnd", attributes: {}, droppedAttributesCount: 0, … }​​
Object { name: "domInteractive", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "domContentLoadedEventStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "domContentLoadedEventEnd", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "domComplete", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "loadEventStart", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "loadEventEnd", attributes: {}, droppedAttributesCount: 0, … }
Object { name: "firstContentfulPaint", attributes: {}, droppedAttributesCount: 0, … }
```

### Events

When you use the login box form input elements, then you should see browswer console events.

WAuth tracks three events:

* onKeyDown

* onKeyUp

* onPaste

The JavaScript code in the file `index.js` has handlers for each of these
events, and prints simple diagnostic information to the browser console.

## TODO: Run Jaeger for distributed telemetry

<https://www.jaegertracing.io/>

Jaeger is an open source distributed tracing platform. It helps monitor and
troubleshoot workflows in distributed systems, such as software applications
that are architected as microservices.
 
Jaeger maps the flow of requests and data as they traverse a distributed system.
These requests may make calls to multiple services, which may introduce their
own delays or errors. Jaeger connects the dots between these disparate
components, helping to identify performance issues, troubleshoot errors, and
improve reliability.

An easy way to run Jaeger is via Docker container:

```sh
docker run --rm --name jaeger \
  -p 16686:16686 \
  -p 4317:4317 \
  -p 4318:4318 \
  -p 5778:5778 \
  -p 9411:9411 \
  jaegertracing/jaeger:2.6.0
```

## Playwright

Run:

```sh
pnpm create playwright
```

Create file:

* [`tests/example.spec.ts`]

Run:

```sh
pnpm exec playwright test
```
