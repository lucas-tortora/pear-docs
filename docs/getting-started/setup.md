# Start and Configure a Pear Desktop Project

This section shows how to generate, configure, and develop a Pear desktop project, in preparation for [Making a Pear Desktop Application](./making-a-pear-desktop-app.md).

<iframe width="560" height="315" src="https://www.youtube.com/embed/y2G97xz78gU?si=4WCsxHmCaXPnDFWh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 1. Create a New Pear Project

### Create a New Directory

and thhs=
Before you begin, create a new directory called `chat` and move into it with the following commands:

```sh
mkdir chat && cd chat
```

### Initialize a Pear Project

You can create a new pear project using the [`pear init`](../references/pear/cli.md#pear-init-flags-linktypedesktop-dir) command. You can add the `--yes` flag so the [Pear CLI](../references/pear/cli.md) uses all the default settings.

```sh
pear init --yes
```

This creates the base project structure.

- `package.json`. App [configuration](../references/pear/configuration.md). Notice the `pear` property.
- `index.html`. App entrypoint.
- `app.js`. Main code.
- `test/index.test.js`. Test skeleton.

## 2. Verify Everything Works

Now, you can use the [`pear run`](..//references/pear/cli.md#pear-run-flags-linkdir-app-args) to verify everything works as expected.

The `--dev` flag enables the browser developer tools to open with the application.

The `.` tells Pear to run the application in the current directory.

```
pear run --dev .
```

![Running pear run --dev .](/img/chat-app-1.png)

## 3. (optional) Enable Automatic Reload

### Update `app.js`

Though this step is not strictly necessary, it's advisable to to enable automatic reloading, so the Pear Runtime live reloads any changes in your app. To do so, you should  add the following lines to `app.js` :

```js
Pear.updates(() => Pear.reload())
```

This line of code tells the [Pear.updates()](../references/pear/api.md#pearupdateslistener-async-functionfunction--streamxreadable) function, that is called for every incoming update with an `update` object, to call [`Pear.reload()`](..//references/pear/api#pearreload), that refreshes desktop applications.

:::warning

Automatic reload is not available for terminal applications.

:::

### Restart Your App

Since your app was already running when you [updated `app.js` to enable live reload](#update-appjs), you will need to stop and restart it for the changes to take effect.

You can kill the process manually by using `ctrl + c` in the terminal that was running it and the restart the app with the following command:

```js
pear run --dev .
```

Now Pear watches project files. When they change, the app is automatically reloaded.

### Ensure Live Reload Works

Keep `pear run --dev .` command running, and open the `index.html` in an editor of your choice and change `<h1>desktop</h1>` to `<h1>Hello world</h1>`.

The app should now show:

![Automatic reload](/img/chat-app-2.png)

> Live reload with hot-module reloading is possible by using the `pear.watch` configuration and the [`pear.updates`](reference/pear/api.md#pear.updates-listener-less-than-async-function-or-function-greater-than-greater-than-streamx.readabl) API. The [pear-hotmods](https://github.com/holepunchto/pear-hotmods) convenience module can also be used.

:::warning Code Changes Only

Changes to your [`package.json` config file](../references/pear/configuration.md) are not reloaded automatically. If you update your config, you should restart your app manually.

:::

## 4. Configure Your App

You can configure your app using the [`package.json`](../references/pear/configuration.md) file.

For example, you can change your app's viewport by editing the `pear.gui` property's `height` and `width`:

```json
{
  ...
  "pear": {
    "gui": {
      "height": 400,
      "width": 700
    }
  }
  ...
}
```

Close the app and re-run `pear run --dev .` to see the changes, the initial window size is different now.

:::tip References

See the [Configuration Documentation](reference/pear/configuration.md) for all options.

:::
