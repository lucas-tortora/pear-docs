# Starting a Pear Terminal Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/UoGJ7PtAwtI?si=daZ_omfb3lDNNDmV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 1. Init

First create a new project using `pear init --type terminal`.

```
mkdir chat-app
cd chat-app
pear init --yes --type terminal
```

This creates the base project structure.

- `package.json`. App configuration. Notice the `pear` property.
- `index.js`. App entrypoint.
- `test/index.test.js`. Test skeleton.

## Step 2. Verify Everything Works

Use `pear run` to see that it works.

```
pear run --dev .
```

> A directory or link needs to be specified with `pear run`, here `.` denotes the current Project directory.

The app will now run. That's all there is to getting a Pear Terminal project started.


## Next

* [Making a Pear Terminal Application](./making-a-pear-terminal-app.md)
