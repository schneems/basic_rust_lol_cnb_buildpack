# Basic Rust CNB

This basic rust CNB should create a web process that echos "lol" in a loop, but it does not.

Compare to a bash example that does the same thing: https://github.com/schneems/basic_bash_lol_cnb_buildpack, (but the bash version works).

## Pre-reqs

- Install rust - [rustup](https://rustup.rs/)
- Add musl target to rustup - `rustup target add x86_64-unknown-linux-musl`
- Install [cargo-make](https://github.com/sagiegurari/cargo-make): `cargo install cargo-make`
- On mac install [homebrew-musl-cross](https://github.com/FiloSottile/homebrew-musl-cross): `brew install FiloSottile/musl-cross/musl-cross`

## Build

```
cargo make pack --profile "production"
pack buildpack package cutlass_local_buildpack_rust_example --config ./target/package.toml --format=image
pack build cutlass_local_rust_example --path . -B heroku/buildpacks:20 --buildpack docker://cutlass_local_buildpack_rust_example  -v
docker run -it --init cutlass_local_rust_example
```

Expected:

```
lol
lol
lol
```

Actual:

```
while true; do echo 'lol'; sleep 2; done: while true; do echo 'lol'; sleep 2; done: command not found
```

## Debug

```
$ pack inspect cutlass_local_rust_example | grep -B2 web
Processes:
  TYPE                 SHELL        COMMAND        ARGS
  web (default)        bash         while true; do echo 'lol'; sleep 2; done
```
