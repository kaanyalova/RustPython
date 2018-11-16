# RustPython
A Python-3  (CPython >= 3.5.0) Interpreter written in Rust :snake: :scream: :metal:.

[![Build Status](https://travis-ci.org/RustPython/RustPython.svg?branch=master)](https://travis-ci.org/RustPython/RustPython)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Contributors](https://img.shields.io/github/contributors/RustPython/RustPython.svg)](https://github.com/RustPython/RustPython/graphs/contributors)
[![Gitter](https://badges.gitter.im/RustPython/Lobby.svg)](https://gitter.im/rustpython/Lobby)

# Usage

To test RustPython, do the following:

    $ git clone https://github.com/RustPython/RustPython
    $ cd RustPython
    $ cargo run demo.py
    Hello, RustPython!

Or use the interactive shell:

    $ cargo run
    Welcome to rustpython
    >>>>> 2+2
    4


# Goals

- Full Python-3 environment entirely in Rust (not CPython bindings)
- A clean implementation without compatibility hacks

# Documentation

Currently the project is in an early phase, and so is the documentation.

You can generate documentation by running:

```shell
$ cargo doc
```

Documentation HTML files can then be found in the `target/doc` directory.

# Code organization

- `parser`: python lexing, parsing and ast
- `vm`: python virtual machine
- `src`: using the other subcrates to bring rustpython to life.
- `docs`: documentation (work in progress)
- `py_code_object`: CPython bytecode to rustpython bytecode convertor (work in progress)
- `wasm`: Binary crate and resources for WebAssembly build 
- `tests`: integration test snippets

# Contributing

To start contributing, there are a lot of things that need to be done.
Most tasks are listed in the [issue tracker](https://github.com/RustPython/RustPython/issues).
Another approach is to checkout the sourcecode: builtin functions and object methods are often the simplest
and easiest way to contribute. 

You can also simply run
`cargo run tests/snippets/todo.py` to assist in finding any
unimplemented method.

# Testing

To test rustpython, there is a collection of python snippets located in the
`tests/snippets` directory. To run those tests do the following:

```shell
$ cd tests
$ pipenv shell
$ pytest -v
```

There also are some unittests, you can run those will cargo:

```shell
$ cargo test --all
```

# Using another standard library

As of now the standard library is under construction.

You can play around
with other standard libraries for python. For example,
the [ouroboros library](https://github.com/pybee/ouroboros).

To do this, follow this method:

```shell
$ cd ~/GIT
$ git clone git@github.com:pybee/ouroboros.git
$ export PYTHONPATH=~/GIT/ouroboros/ouroboros
$ cd RustPython
$ cargo run -- -c 'import statistics'
```

# Compiling to WebAssembly

At this stage RustPython only has preliminary support for web assembly. The instructions here are intended for developers or those wishing to run a toy example.

## Setup

To get started, install [wasm-bindgen](https://rustwasm.github.io/wasm-bindgen/whirlwind-tour/basic-usage.html)
and [wasm-pack](https://rustwasm.github.io/wasm-pack/installer/). You will also need to have `npm` installed.

<!-- Using `rustup` add the compile target `wasm32-unknown-emscripten`. To do so you will need to have [rustup](https://rustup.rs/) installed.

```bash
rustup target add wasm32-unknown-emscripten
```

Next, install `emsdk`:

```bash
curl https://s3.amazonaws.com/mozilla-games/emscripten/releases/emsdk-portable.tar.gz | tar -zxv
cd emsdk-portable/
./emsdk update
./emsdk install sdk-incoming-64bit
./emsdk activate sdk-incoming-64bit
``` -->



## Build

Move into the `wasm` directory. This contains a custom library crate optimized for wasm build of RustPython.   

```bash
cd wasm
```

From here run the build. This can take several minutes depending on the machine.

```
wasm-pack build
```

Upon successful build, cd in the the `/pkg` directory and run:

```
npm link
```

Now move back out into the `/app` directory. The files here have been adapted from [wasm-pack-template](https://github.com/rustwasm/wasm-pack-template).

Finally, run:

```
npm install
npm link rustpython_wasm
```

and you will be able to run the files with:

```
node_modules/.bin/webpack-dev-server
```

Open a browser console and see the output of rustpython_wasm. To verify this, modify the line in `app/index.js`

```js
rp.run_code("print('Hello Python!')\n");
```

To the following:

```js
rp.run_code("assert(False)\n");
```

and you should observe: `Execution failed` in your console output, indicating that the execution of RustPython has failed.

# Code style

The code style used is the default rustfmt codestyle. Please format your code accordingly.

# Community

Chat with us on [gitter][gitter].

# Credit

The initial work was based on [windelbouwman/rspython](https://github.com/windelbouwman/rspython) and [shinglyu/RustPython](https://github.com/shinglyu/RustPython)

[gitter]: https://gitter.im/rustpython/Lobby

# Links

These are some useful links to related projects:

- https://github.com/ProgVal/pythonvm-rust
- https://github.com/shinglyu/RustPython
- https://github.com/windelbouwman/rspython

