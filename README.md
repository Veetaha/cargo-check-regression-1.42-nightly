# Cargo check regression report

This is a bug report originally found [here](https://github.com/rust-analyzer/rust-analyzer/issues/2973) and reported to cargo team [here](https://github.com/rust-lang/cargo/issues/7861).

# Steps to reproduce

Make sure to install these versions of cargo:

* `cargo 1.42.0-nightly (9d32b7b01 2020-01-26)` - referred as **nightly**

* `cargo 1.41.0 (626f0f40e 2019-12-03)` - referred as **stable**

```
rustup default nightly
cargo check --message-format json > check-1.42.json

rustup default stable
cargo check --message-format json > check-1.41.json
```

You can see the difference in spans for an empty main.rs error message.
Stable version returns no error spans, but nightly one returns an array of one
span where `line_start` and `line_end` are set to 0, though [they are guaranteed
to be 1-based](https://github.com/oli-obk/cargo_metadata/blob/master/src/diagnostic.rs#L58-L61).
