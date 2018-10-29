# Cargo-sysroot

[![Crates.io](https://img.shields.io/crates/v/cargo-sysroot.svg)](https://crates.io/crates/cargo-sysroot)

A (dumb) tool to compile libcore and friends for no_std crates.

This is not a wrapper like `cargo xbuild` or `xargo`, this is a standalone tool you call once.

## Prerequisite

* A nightly compiler.
* The `rust-src` component must be installed for the active toolchain.
* Your `Cargo.toml` file must contain `package.metadata.cargo-sysroot.target`, where target is a target specifiction json file.
    * A rust supported target may also work, but this is untested.

### Example `Cargo.toml`

```toml
[package]
name = "My Project"
version = "0.1.0"
authors = ["Me <Me@Me.com>"]

[package.metadata.cargo-sysroot]
target = "my_custom_target.json" # This is relative to Cargo.toml
```

## Getting Started

* Run `cargo install cargo-sysroot`.
* Run `cargo sysroot` in the working directory of your project.

This tool will generate a `.cargo/config` for you that looks something like this

```toml
[build]
target = "path/to/your/target/specification/json"
rustflags = [
    "--sysroot",
    "full/path/to/target/sysroot",
]
```

The sysroot will be located at `target/sysroot` and the libcore target directory at `target/sysroot/target`.
Multiple target specifictions are supported and will not conflict, and due to the way rust uses the sysroot your generated `.cargo/config`
should not need to be changed, as the base path it uses is not target specific.

Note that this tool is currently quite stupid, so it won't attempt to do anything if that file already exists.

This will allow Cargo to properly build your project with the normal commands such as `cargo build`.
You may wish to modify this file to make use of the `target.$triple.runner` key. See the [Cargo Documentation](https://doc.rust-lang.org/cargo/reference/config.html#configuration-keys) for details.
Note that the author experienced problems with the $triple variant not working, and you may experience better success with the cfg variant.

If you update your Rust nightly version you will need to run `cargo-sysroot` again.
Note that doing this will cause cargo to detect that libcore has changed and rebuild your entire project.

## Limitations

* Liballoc is currently unsupported.

## License

Licensed under either of

* Apache License, Version 2.0
   ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0)>
* MIT license
   ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT)>

at your option.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
