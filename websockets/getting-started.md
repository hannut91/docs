# Getting started

## Installation

In order to work with _WebSockets_ module you need to install WebSocket module:

```bash
$ npm i @marblejs/websockets
```

or if you are a hipster:

```bash
$ yarn add @marblejs/websockets
```

## Bootstrapping

Like [_httpListener_](../api-reference/core/core-httplistener.md) __the WebSocket module defines a similar way for bootstrapping. The [_webSocketListener_](../api-reference/websockets/websocketlistener.md) includes definitions of all _middlewares_ and WebSocket _effects_.

{% code-tabs %}
{% code-tabs-item title="webSocket.listener.ts" %}
```typescript
const effects = [
  effect1$,
  effect2$,
  // ...
];

const middlewares = [
  middleware1$,
  middleware2$,
  // ...
];

export default webSocketListener({ effects, middlewares });
```
{% endcode-tabs-item %}
{% endcode-tabs %}

To connect the previously configured WebSocket listener, you have to create a context token first.

{% code-tabs %}
{% code-tabs-item title="tokens.ts" %}
```typescript
import { createContextToken } from '@marblejs/core';
import { MarbleWebSocketServer } from '@marblejs/websockets';

export const WebSocketServerToken = createContextToken<MarbleWebSocketServer>();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
You can learn more about Marble.js Context mechanism [here](../advanced/context.md).
{% endhint %}

Then all you have to do is to register the defined module inside _createServer_ `dependencies`.

{% code-tabs %}
{% code-tabs-item title="index.ts" %}
```typescript
import { bindTo createServer } from '@marblejs/core';
import { WebSocketServerToken } from './tokens.ts';
import httpListener from './http.listner.ts';
import webSocketListener from './webSocket.listner.ts';

const server = createServer({
  // ...
  httpListener,
  dependencies: [
    bindTo(WebSocketServerToken)(webSocketListener({ port: 8080 }).run),
  ],
  // ...
});

server.run();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You can create and run a WebSocket server by providing the port value in `webSocketListener` config object. You can also upgrade the currently running http server by providing `noServer: true` in config object or by ommiting it.

{% hint style="info" %}
If you are curious about other ways of bootstrapping the WebSocket server, reach out the [Server events](../advanced/server-events.md) chapter.
{% endhint %}

