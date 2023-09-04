---
title: CakeML
lang: en
header-includes:
  - |
    <style type="text/css"> body {font-family: Arial, Helvetica; margin-left: 5em; font-size: large;} </style>
    <style type="text/css"> h1 {margin-left: 0em; padding: 0px; text-align: center} </style>
    <style type="text/css"> h2 {margin-left: 0em; padding: 0px; color: #580909} </style>
    <style type="text/css"> h3 {margin-left: 1em; padding: 0px; color: #C05001;} </style>
    <style type="text/css"> body { width: 1100px; margin-left: 30px; }</style>
---

## CakeML Mac instructions

Here are the full instructions to get a native ARM executable of a CakeML "Hello World!" program on Apple Silicon (M1 Pro):

### Setup

Download an `x86-64` [CakeML release](https://github.com/CakeML/cakeml/releases).

On `x86-64` (or using [Rosetta 2](https://support.apple.com/en-gb/guide/security/secebb113be1/web), e.g. via `arch -x86_64 /bin/zsh`):
```sh
# Link cake.S against basis library and produce executable ./cake
make cake

# Cross compile the compiler for arm8
time env CML_HEAP_SIZE=4096 CML_STACK_SIZE=4096 \
  ./cake --target=arm8 --sexp=true --jump=false \
    --skip_type_inference=true --exclude_prelude=true \
     < cake-sexpr-64 > cake-arm8.S
```

On `arm8` with some C compiler:
```sh
# Link cake-arm8.S against basis library and produce executable
clang cake-arm8.S basis_ffi.c -o cake-arm8
```

### Usage

```sh
./cake-arm8 --target=arm8 < hello.cml > hello.S

# Link hello.S against basis library and produce executable
clang hello.S basis_ffi.c -o hello.cake

# Run executable

./hello.cake # Hello, World!
```

The `--target=arm8` argument is still necessary, [since the CakeML compiler defaults to an `x86` target](https://github.com/CakeML/cakeml/blob/a500b998760b855e6d32428c2e39ce0f69a89131/compiler/compile)
