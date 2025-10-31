# Usage

## Install Bare

You can install Bare using the following command:

```sh
npm i -g bare
```
## Basic Usage

```console
bare [flags] <filename> [...args]

Evaluate a script or start a REPL session if no script is provided.

Arguments:
  <filename>            The name of a script to evaluate
  [...args]             Additional arguments made available to the script

Flags:
  --version|-v          Print the Bare version
  --eval|-e <script>    Evaluate an inline script
  --print|-p <script>   Evaluate an inline script and print the result
  --inspect             Activate the inspector
  --help|-h             Show help
```

The specified `<script>` or `<filename>` is run using `Module.load()`. For more information on the module system and the supported formats, see [BareModule](https://github.com/holepunchto/bare-module).

## API

Bare makes it easy to craft applications that can run effectively across a broad spectrum of devices. To get started, find the Bare API specs [here](./api.md).

### Modules

Bare provides no standard library beyond the core JavaScript API available through the `Bare` namespace. Instead, there is a comprehensive collection of external modules built specifically for Bare, see [Bare Modules](./bare-modules.md)

### Embedding

Bare can easily be embedded using the C API defined in [`include/bare.h`](https://github.com/holepunchto/bare/blob/main/include/bare.h):

```c
#include <bare.h>
#include <uv.h>

bare_t *bare;
bare_setup(uv_default_loop(), platform, &env /* Optional */, argc, argv, options, &bare);

bare_load(bare, filename, source, &module /* Optional */);

bare_run(bare);

int exit_code;
bare_teardown(bare, &exit_code);
```

If `source` is `NULL`, the contents of `filename` will instead be read at runtime. For examples of how to embed Bare on mobile platforms, see [Bare Android](https://github.com/holepunchto/bare-android) and [Bare iOS](https://github.com/holepunchto/bare-ios).

### Suspension

Bare provides a mechanism for implementing process suspension, which is needed for platforms with strict application lifecycle constraints, such as mobile platforms. When suspended, a `suspend` event will be emitted on the `Bare` namespace. Then, when the loop has no work left and would otherwise exit, an `idle` event will be emitted and the loop blocked, keeping it from exiting. When the process is later resumed, a `resume` event will be emitted and the loop unblocked, allowing it to exit when no work is left.

The suspension API is available through `bare_suspend()` and `bare_resume()` from C and `Bare.suspend()` and `Bare.resume()` from JavaScript.

## Building

[`BareMake`](https://github.com/holepunchto/bare-make) is used for compiling Bare. Start by installing the tool globally:

```console
npm i -g bare-make
```

Next, install the required build and runtime dependencies:

```console
npm i
```

Then, generate the build system:

```console
bare-make generate
```

This only has to be run once per repository checkout. When updating `bare-make` or your compiler toolchain it might also be necessary to regenerate the build system. To do so, run the command again with the `--no-cache` flag set to disregard the existing build system cache:

```console
bare-make generate --no-cache
```

With a build system generated, Bare can be compiled:

```console
bare-make build
```

When completed, the `bare(.exe)` binary will be available in the `build/bin` directory and the `libbare.(a|lib)` and `(lib)bare.(dylib|dll|lib)` libraries will be available in the root of the `build` directory.

### Linking

When linking against the static `libbare.(a|lib)` library, make sure to use whole archive linking as Bare relies on constructor functions for registering native addons. Without whole archive linking, the linker will remove the constructor functions as they aren't referenced by anything.

### Options

Bare provides a few compile options that can be configured to customize various aspects of the runtime. Compile options may be set by passing the `--define option=value` flag to the `bare-make generate` command when generating the build system.

> [!WARNING]  
> The compile options are not covered by semantic versioning and are subject to change without warning.

| Option           | Default                    | Description                                             |
| :--------------- | :------------------------- | :------------------------------------------------------ |
| `BARE_ENGINE`    | `github:holepunchto/libjs` | The JavaScript engine to use                            |
| `BARE_PREBUILDS` | `ON`                       | Enable prebuilds for supported third-party dependencies |
