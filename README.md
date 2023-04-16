# Just for static file hoisting powered by [cloudfare works](https://www.cloudflare.com/)

## Setup 

### 1. Create a worker

```js
const targetUrl = 'https://raw.githubusercontent.com/[your-github-username]/[your-repositry-name]/[your-branch-name]'
// such as https://raw.githubusercontent.com/guygubaby/static/main

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);
  const newUrl = targetUrl + url.pathname

  const modifiedRequest = new Request(newUrl, {
    headers: request.headers,
    method: request.method,
    body: request.body,
    redirect: 'follow'
  });

  const response = await fetch(modifiedRequest);
  const modifiedResponse = new Response(response.body, response);

  modifiedResponse.headers.set('Access-Control-Allow-Origin', '*');
  response.ok && modifiedResponse.headers.set('origin-url', newUrl);

  return modifiedResponse;
}
```

### 2. And then bind your own domain to the worker (Optional)

After bind your own domain to the worker, you can use the domain to access the static file. And also can access the static file from mainland China without proxy.

## Usage

```ts
const BaseUrl = 'https://static.guygubaby.top'

// All assets are file based routing
const README_URL = `${BaseUrl}/README.md`
```
