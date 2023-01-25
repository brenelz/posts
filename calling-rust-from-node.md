---
title: "Call Rust From Node"
date: "2022-02-14"
slug: "calling-rust-from-node"
---

Lately I've been a bit interested in Rust. I did a project at work to speed up
our build and SWC (written in Rust) was the answer.

I took it upon myself to see how easy it is to call into Rust using Node. It
took a bit of trial an error but I got a super basic example working that I will
share with you.

You can view the full repo on
<a href="https://github.com/brenelz/call-rust-from-node">my github</a>.

## Prerequisites

&bull; <a href="https://nodejs.org/en/download/">Node</a><br /> &bull;
<a href="https://www.rust-lang.org/tools/install">Rust</a>

## The Rust Part

Let's start with the Rust part. First you will want to create a directory to
house our project.

```
mkdir call-rust-from-node
cd call-rust-from-node
```

Then create a `Cargo.toml` file:

```
[package]
name = "call-rust-from-node"
edition = "2021"
version = "0.1.0"

[lib]
crate-type = ["cdylib"]

[dependencies]
napi = {version = "2"}
napi-derive = {version = "2"}

[build-dependencies]
napi-build = "1"
```

I'm not too familiar with everything but the important part is the name,
crate-type, and dependencies. We are using
<a href="https://github.com/napi-rs/napi-rs">napi-rs</a> to help us with some of
the plumbing.

Next create a file called `build.rs`:

```
extern crate napi_build;

fn main() {
    napi_build::setup();
}
```

This is needed for the napi build process to work.

Next in a `src` folder create a `lib.rs` file:

```
#![allow(unused_imports)]
#[macro_use]
extern crate napi_derive;

use napi::bindgen_prelude::*;

#[napi]
pub fn sum(a: u32, b: u32) -> u32 {
    a + b
}
```

The actually function we are exposing to node is a `sum` function which takes
two numbers and adds them together.

Take note of the `#[napi]` syntax as well as the `pub fn sum`.

Believe it or not thats all the Rust we are going to write today. You can take
this further by
<a href="https://github.com/napi-rs/napi-rs/tree/main/examples/napi/src">exploring
more examples</a> of how to expose certain things from Rust.

## The Node Part

Create a `package.json` in the root as follows:

```
{
  "package": "call-rust-from-node",
  "devDependencies": {
    "@napi-rs/cli": "^1.0.0"
  },
  "napi": {
    "name": "call-rust-from-node"
  },
  "scripts": {
    "build": "napi build --release"
  }
}
```

The import bits here are the `devDependencies`, the `napi` name, and the `build`
script. The `napi` name shown here is what we will use to call into Rust.

Next run an `npm install` to make sure you have things installed.

After that it is the moment of truth. Run an `npm run build` and hope you don't
get any cryptic errors.

You will notice it has created a `call-rust-from-node.node` file. We can now
create and call an index.js file.

```
const { sum } = require("./call-rust-from-node.node");

console.log(sum(2, 5));
```

```
node index.js
```

Boom! You should see the output of `7`.

## Conclusion

Obviously this is a super basic example and thus the speed of Rust doesn't get
to show. Imagine the possibilities for more complex things like transpiling,
bundling, etc. This is the reason Rust is slowly taking over JS tooling.

I hope you learned something, and until next time!
