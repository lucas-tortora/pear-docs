# Create a Pear Desktop Application

If you haven't done so already, please [set up your first Pear Application](setup.md), as this guide builds on that.

<iframe width="560" height="315" src="https://www.youtube.com/embed/y2G97xz78gU?si=E8UmXSS2WWYtIV1V" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 1. Update HTML and CSS

:::tip Frontend Frameworks

You can use any frontend framework alongside Pear.

:::

So far, you've only [updated the HTML to say `Hello World!` in the previous example](setup.md#ensure-live-reload-works).
Since this tutorial aims to create a chat application, you'll need to update the HTML and CSS to define your app's layout by updating the `index.html` file:

```html reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/index.html
```

If you [enabled automatic reloading in your `app.js` file](setup.md#3-optional-enable-automatic-reload), and your app is running, you should see your changes.

If not, stop your app and restart it by calling `pear run --dev .`.

![Layout of the app](/img/chat-app-3.png)

:::note Pear Ctrl

To make the `<pear-ctrl>` element draggable in Pear applications, wrap it in another element that uses the following CSS property:

```css
-webkit-app-region: drag;
```

This non-standard CSS property tells the application that this element should act as a draggable region for the entire window.

:::

## 2. Install Dependencies

:::warning Close Your App

Pear will throw an error if you install dependencies while your app is running. Please close it and restart the app after installing.

:::

### Dev Dependencies

The simple chat app in this tutorial requires the following dependencies, which are already required in your `package.json` file:

```json reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/package.json#L19-L22
```

You can install them by running the following command:

```bash
npm install
```

This will install the following:

- [pear-interface](https://github.com/holepunchto/pear-interface) for documentation and auto-completion inside editor.
- [brittle](https://github.com/holepunchto/brittle) a TAP framework for testing.

### Used Modules

The app uses these modules:

- [hyperswarm](https://www.npmjs.com/package/hyperswarm) to connect peers on a "topic".
- [hypercore-crypto](https://www.npmjs.com/package/hypercore-crypto) for basic cryptography.
- [b4a](https://www.npmjs.com/package/b4a) to manipulate buffers.

Install the dependencies with:

```bash
npm install hyperswarm hypercore-crypto b4a
```

If you have installed them correctly, you should now see them in your `package.json` file under `dependencies`:

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/package.json#L23-L27
```

## 3. Code Your App

Once you've set up your [HTML and CSS](#1-update-html-and-css) and [installed any necessary dependencies](#2-install-dependencies) you can start to add the logic for your app in the `app.js` file.

### Add Documentation and Code Completion

The first thing you should do, is add a `@typedef` to import the `pear-interface`. This will help out by adding interactive documentation and code-completion in your editor:

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L3
```

### Add Globals and Imports

Next, you should declare a global `Pear` object and destructure `teardown` and `updates` from the global `Pear` API, and import the following modules that will be used by the app:

- [`hyperswarm`](https://github.com/holepunchto/hyperswarm) for P2P networking.
- [`hypercore-crypto`](https://github.com/holepunchto/hypercore-crypto) for random bytes
- [`b4a`](https://www.npmjs.com/package/b4a) for buffer conversions.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L2-L9
```

### Create a HyperSwarm Instance

The first thing you should do in your chat app is create a `HyperSwarm` instance called `swarm`, that will manage peer discovery and connections:

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L10-L11
```

### Manage Teardown and Updates

You can now tell your `HyperSwarm` instance how you want it to handle [`updates`](../references/pear/api.md#pearupdateslistener-async-functionfunction--streamxreadable), and the [`teardown`](../references/pear/api.md#pearteardownfn-async-functionfunction) event.

#### Teardown

The [`teardown`](../references/pear/api.md#pearteardownfn-async-functionfunction) function manages application cleanup handlers. In a nutshell, it indicates what will happen when the application begins to unload. In our case, you should add the following line that will "unannounce", i.e. destroy, the chat room. This is not strictly necessary, but it helps to keep the distributed hash table (DHT) small.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L15
```

#### Updates

The [`updates`](../references/pear/api.md#pearupdateslistener-async-functionfunction--streamxreadable) listener function is called for every incoming `update` object. In this case, you should add a call to [`Pear.reload()`](../references/pear/api.mdx#pearreload)

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L19
```

### Handle Incoming Connections

You should now tell your `swarm` instance how to handle incoming connections. The following code will run when a new peer connects. It will:

1. Creater a `name` for the peer using the first 6 hex characters of its public key.
2. Listen for `data` events and call the `onMessageAdded` for each.
3. Listen for `errors` and log them to the console.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L22-L27
```

### Update Peer Count On Swarm Changes

You can also add a peer count by telling your `swarm` instance how it should have a new connection event using the [swarm.on()](../references/building-blocks/hyperswarm#events) function and passing `connection` as the event.
This snippet will update the current number of active connections into the `#peers-count` DOM element.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L30-L32
```

### Create DOM Event Listeners

The buttons you created in [your HTML](#1-update-html-and-css) don't have any listeners yet, which means they don't really do anything at the moment.

The following lines will use the add an event listener that will call functions to:

- [Create a chat room](#createchatroom) when the `#create-chat-room` form is submitted:

  ```js reference
  https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L34
  ```

- Allow user to [join a chat room](#joinchatroom) when the `#join-form` is submitted:

  ```js reference
  https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L35
  ```

- [Send a message](#sendmessage) when the `#message-form` is submitted:

  ```js reference
  https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L36
  ```

  Attaches DOM event listeners to the UI buttons and forms: creatin

### Event Functions

So far, you've added event listeners. But, as you may have noticed, you haven't defined the functions that will be called by the events.
You should add each of these functions to your `app.js` file

#### `createChatRoom`

Generates a random 32-byte topic using `crypto.randomBytes` and calls [`joinSwarm`](#joinswarm) with the created topic buffer.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L38-L42
```

#### `joinChatRoom`

Prevents the form's default submit behavior, reads the hex topic string from the input field, converts it into a buffer with `b4a.from`, and calls [`joinSwarm`](#joinswarm)with that buffer.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L44-L49
```

#### `JoinSwarm`

Hides the setup UI, shows a loading state, joins the swarm using `swarm.join(topicBuffer, { client: true, server: true })`, waits for discovery to flush, then displays the chat UI and the topic hex string.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L51-L63
```

:::info

In a [traditional client-server setup](../about-pear/what-is-pear.md#web2), the server is hosted at an IP address (or hostname) and a port, e.g. `http://localhost:3000`. This is what clients use to connect to the server.

The code in `app.js` contains the line `swarm.join(topicBuffer, { client: true, server: true })`.

`topicBuffer` is the invitation, meaning that anyone who knows this topic can join the room and message the other members.

There is no separate client or server, all members are equal. If the room creator goes offline, or even deletes the room from their machine, the other members can continue chatting.

:::

#### `sendMessage`

Reads the message text from the input, clears the input, prevents form submission from reloading the page, appends the message locally (`onMessageAdded('You', message)`), and writes the message to every currently connected peer.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L65-L75
```

#### `onMessageAdded`

Creates a new `div` element, sets its text to `<from> message`, and appends it to the `#messages` list in the DOM.

```js reference
https://github.com/lucas-tortora/pear-chat-app/blob/main/app.js#L78-L82
```

:::note

Note that the `pear` dependency is used, even though it was not installed. This is the [Pear API](reference/pear/api.md), available to any Pear project.

:::

## 4. Chat

After you have [coded your app](#3-code-your-app), you can start chatting with yourself:

1. Open two terminal windows and navigate to the app's directory.
2. In each, run the `pear run --dev .` command.

In the first app, click on `Create`. A random topic will appear at the top.

Note that topics consist of 64 hexadecimal characters (32 bytes).

![The first app, with the topic](/img/chat-app-4a.png)

Paste the topic into the second app, then click on `Join`.

![Second app, using topic from the first](/img/chat-app-4b.png)

Once connected, messages can be sent between each chat application.

![View from the second app](/img/chat-app-5b.png)

Of course, a chat app that only runs on one computer isn't particularly useful. The next step in this guide will show you how to [release](release.md) and [share](share.md) your app.