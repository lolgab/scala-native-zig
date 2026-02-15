# Scala Native Cross-Compilation with Zig

This example shows how to cross-compile [Scala Native](https://scala-native.org/) binaries using the [Zig](https://ziglang.org/) toolchain as a drop-in replacement for Clang.

By leveraging Zig's built-in cross-compilation capabilities, you can build native binaries for multiple platforms from a single machine — no need to install separate cross-compilers for each target.

## Supported Targets

The project cross-compiles to the following targets:

| Target | OS | Architecture |
|---|---|---|
| `x86_64-linux-gnu` | Linux | x86_64 |
| `aarch64-linux-gnu` | Linux | AArch64 |
| `x86_64-macos-none` | macOS | x86_64 |
| `aarch64-macos-none` | macOS | AArch64 |
| `x86_64-windows-gnu` | Windows | x86_64 |
| `aarch64-windows-gnu` | Windows | AArch64 |

## How It Works

The [build.mill](build.mill) configuration overrides `nativeClang` and `nativeClangPP` to delegate to `zig cc` and `zig c++` respectively, which act as drop-in replacements for `clang` / `clang++` with cross-compilation support out of the box. The `nativeTarget` is set from the cross-compilation matrix, so Mill builds one binary per target.

## Prerequisites

- [Zig](https://ziglang.org/download/) (tested with 0.15.2)
- JDK 11+ (for running Mill and the Scala compiler)

## Usage

Build all targets:

```bash
./mill __.nativeLink
```

Build a specific target:

```bash
./mill app[aarch64-linux-gnu].nativeLink
```

The output binaries are placed under `out/app/<target>/nativeLink.dest/`.

## CI / Release

A [GitHub Actions workflow](.github/workflows/release.yml) is included that builds all six targets on Ubuntu using Zig and creates a GitHub Release with the resulting binaries whenever a version tag (`v*`) is pushed.
