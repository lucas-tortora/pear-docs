# Platform Support

Bare uses a tiered support system to manage expectations for the platforms that it targets. Targets may move between tiers between minor releases and as such a change in tier will not be considered a breaking change.

## Tier 1

Platform targets for which prebuilds are provided as defined by the [`.github/workflows/prebuild.yml`](https://github.com/holepunchto/bare/blob/main/.github/workflows/prebuild.yml) workflow. Compilation and test failures for these targets will cause workflow runs to go red.

## Tier 2

Platform targets for which Bare is known to work, but without automated compilation and testing. Regressions may occur between releases and will be considered bugs.

> [!NOTE]  
> Development happens primarily on Apple hardware with Linux and Windows systems running as virtual machines.

| Platform  | Architecture | Version                              | Tier | Notes                       |
| :-------- | :----------- | :----------------------------------- | :--- | :-------------------------- |
| GNU/Linux | `arm64`      | >= Linux 5.15, >= GNU C Library 2.35 | 1    | Ubuntu 22.04, OpenWrt 23.05 |
| GNU/Linux | `x64`        | >= Linux 5.15, >= GNU C Library 2.35 | 1    | Ubuntu 22.04, OpenWrt 23.05 |
| GNU/Linux | `arm64`      | >= Linux 5.10, >= musl 1.2           | 2    | Alpine 3.13, OpenWrt 22.03  |
| GNU/Linux | `x64`        | >= Linux 5.10, >= musl 1.2           | 2    | Alpine 3.13, OpenWrt 22.03  |
| GNU/Linux | `mips`       | >= Linux 5.10, >= musl 1.2           | 2    | OpenWrt 22.03               |
| GNU/Linux | `mipsel`     | >= Linux 5.10, >= musl 1.2           | 2    | OpenWrt 22.03               |
| Android   | `arm`        | >= 9                                 | 1    |
| Android   | `arm64`      | >= 9                                 | 1    |
| Android   | `ia32`       | >= 9                                 | 1    |
| Android   | `x64`        | >= 9                                 | 1    |
| macOS     | `arm64`      | >= 11.0                              | 1    |
| macOS     | `x64`        | >= 11.0                              | 1    |
| iOS       | `arm64`      | >= 14.0                              | 1    |
| iOS       | `x64`        | >= 14.0                              | 1    | Simulator only              |
| Windows   | `arm64`      | >= Windows 11                        | 1    |
| Windows   | `x64`        | >= Windows 10                        | 1    |
