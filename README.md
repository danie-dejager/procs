# procs

**procs** is a replacement for `ps` written by [Rust](https://www.rust-lang.org/).

[![Build Status](https://travis-ci.org/dalance/procs.svg?branch=master)](https://travis-ci.org/dalance/procs)
[![Crates.io](https://img.shields.io/crates/v/procs.svg)](https://crates.io/crates/procs)
[![codecov](https://codecov.io/gh/dalance/procs/branch/master/graph/badge.svg)](https://codecov.io/gh/dalance/procs)

## Features

- Output by the colored and human-readable format
- Keyword search over multi-column
- Some additional information (ex. TCP/UDP port, Read/Write throughput) which are not supported by `ps`

## Platform

Linux is supported only.

## Installation

### Download binary

Download from [release page](https://github.com/dalance/procs/releases/latest), and extract to the directory in PATH.

### Cargo

You can install by [cargo](https://crates.io).

```
cargo install procs
```

## Usage

Type `procs` only. It shows the information of all processes.

```console
$ procs
```

![procs](https://user-images.githubusercontent.com/4331004/51976289-9f3a4a00-24c7-11e9-8573-8f746ccf1ed4.png)

If you add any keyword as argument, it is matched to `USER` or `Command` by default.

```console
$ procs zsh
```

![procs_zsh](https://user-images.githubusercontent.com/4331004/51976319-b24d1a00-24c7-11e9-8060-09e71d18e5ec.png)

If a numeric is used as the keyword, it is matched to `PID`, `TCP`, `UDP` by default.
Numeric is treated as exact match, and non-numeric is treated as partial match by default.

```console
$ procs 6000 60000 60001 16723
```

![procs_port](https://user-images.githubusercontent.com/4331004/51976347-c09b3600-24c7-11e9-8d40-2c437efbd5e1.png)

Note that procfs permissions only allow identifying listening ports for processes owned by the current user, so not all ports will show up unless run as root.

## Configuration

You can change configuration by `~/.procs.toml` like below.
The complete example of `~/.procs.toml` can be generated by `--config` option.

```toml
[[columns]]
kind = "Pid"
style = "BrightYellow"
numeric_search = true
nonnumeric_search = false

[[columns]]
kind = "Username"
style = "BrightGreen"
numeric_search = false
nonnumeric_search = true

[style]
header = "BrightWhite"
unit = "BrightWhite"

[style.by_percentage]
color_000 = "BrightBlue"
color_025 = "BrightGreen"
color_050 = "BrightYellow"
color_075 = "BrightRed"
color_100 = "BrightRed"

[style.by_state]
color_d = "BrightRed"
color_r = "BrightGreen"
color_s = "BrightBlue"
color_t = "BrightCyan"
color_z = "BrightMagenta"
color_x = "BrightWhite"

[style.by_unit]
color_k = "BrightBlue"
color_m = "BrightGreen"
color_g = "BrightYellow"
color_t = "BrightRed"
color_p = "BrightRed"
color_x = "BrightBlue"

[search]
numeric_search = "Exact"
nonnumeric_search = "Partial"

[sort]
column = 0
order = "Ascending"
```

`[[columns]]` section defines which columns are used.
The first `[[columns]]` is shown at left side, and the last is shown at right side.
`kind` is column type and `style` is column color.
`numeric_search` and `nonnumeric_search` mean whether this column can be matched by numeric/non-numeric search keyword.
The available list of `kind` and `style` is below.

There are some special styles like `ByPercentage`, `ByState`, `ByUnit`.
These are the styles for value-aware coloring.
For example, if `ByUnit` is choosen, color can be specified for each unit of value ( like `K`, `M`, `G`,,, ).
The colors can be configured in `[style.by_unit]` section.

`[style]` section defines colors of header and unit line.
`[search]` section defines match policy. Policy can be `Exact` or `Partial`.
`[sort]` section defines the column used for sort and sort order.
`order` can be `Ascending` or `Descending`.

### `kind` list

| procs `kind` | `ps` STANDARD FORMAT  |
| ------------ | --------------------- |
| Command      | args                  |
| CpuTime      | cputime               |
| Docker       | -not supported-       |
| Nice         | ni                    |
| Pid          | pid                   |
| ReadBytes    | -not supported-       |
| Separator    | -not supported-       |
| StartTime    | start_time            |
| State        | s                     |
| TcpPort      | -not supported-       |
| Tty          | tty                   |
| UdpPort      | -not supported-       |
| UsageCpu     | %cpu                  |
| UsageMem     | %mem                  |
| Username     | euser                 |
| VmRss        | rss                   |
| VmSize       | vsz                   |
| WriteByte    | -not supported-       |

### `style` list

- BrightRed
- BrightGreen
- BrightYellow
- BrightBlue
- BrightMagenta
- BrightCyan
- BrightWhite
- Red
- Green
- Yellow
- Blue
- Magenta
- Cyan
- White
- ByPercentage
- ByState
- ByUnit


