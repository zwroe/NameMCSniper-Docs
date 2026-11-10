# Installation

## Open a Windows build

Double-click the `namemcsniper.exe` supplied with your Rust build to open the guided terminal menu. Choose **3 · Try an offline simulation** to test it without signing in to Minecraft.

If you use PowerShell, open it in the folder containing the executable.

```powershell
.\namemcsniper.exe --help
.\namemcsniper.exe simulate
```

## Build from source

Use the NameMCSniper Rust checkout containing `Cargo.toml` and `Cargo.lock`. Install stable Rust with rustup and follow its prompts for the Visual Studio C++ build tools on Windows. [Rust installation guide](https://rust-lang.org/tools/install/)

On Debian or Ubuntu, install `pkg-config` and `libssl-dev` before building.

From the Rust checkout, build the executable.

```sh
cargo build --locked --release
```

On Windows, open `target\release\namemcsniper.exe`. On Linux or macOS, run the executable from the checkout.

```sh
./target/release/namemcsniper
```

To install `namemcsniper` on your command path, run `cargo install --locked --path .` from the Rust checkout.

## First run

Start with the simulation, then use the [guided menu](../usage/basic-usage.md) to look up a name or preview a schedule. Set up [authentication](authentication.md) when you want to check an account or prepare a live rename.
