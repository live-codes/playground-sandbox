# Playground Sandbox

This repo contains a web page that can be used as a sandbox for running untrusted code in playgrounds (similar to [livecodes.io](https://livecodes.io)).

## Usage

The page "[index.html](./index.html)" is used as the `src` for a [sandboxed iframe](https://web.dev/articles/sandboxed-iframes) in which untrusted code can be executed. This page has to be on a different domain than the playground. That's why it is published to npm, so that it can be hosted on CDNs (e.g. unpkg).

The html is sent to the iframe using [`postMessage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) in an object with the `html` key.

Example:

```js
const iframe = document.createElement("iframe");
iframe.src =
  "https://unpkg.com/@live-codes/playground-sandbox@1.0.0/index.html";
iframe.setAttribute(
  "sandbox",
  "allow-same-origin allow-forms allow-scripts allow-modals" // and whatever else you need
);
iframe.onload = function () {
  iframe.contentWindow.postMessage(
    {
      html: `
        <h1>hello world!</h1>
        I do not have access to the parent window except through \`postMessage\`.
    `,
    },
    "*"
  );
};
document.body.appendChild(iframe);
```

## License

MIT License
