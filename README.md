# esy-npm-project-example
The simplest esy project, installable with the npm CLI.


[![Build Status](https://travis-ci.org/reasonml/esy-npm-project-example.svg?branch=master)](https://travis-ci.org/reasonml/esy-npm-project-example)


This project has nothing to do with Reason, and nothing to do with ocaml.
The only purpose is to demonstrate that [`esy`](https://github.com/reasonml/esy)
can be used with pure NPM packages.


## Try:
```
git clone git@github.com:reasonml/esy-npm-project-example.git
cd esy-npm-project-example
npm install
```

### Verify that `esy` ran its build
```
ls _install
```


This project merely demonstrates using `esy` to perform the simplest possible
"native build/compile". Native compilation "installs" a subset of artifacts
into a final destination, and this project demonstrates doing that.

As a result of using `esy`, this tiny project benefits from everything that
`esy` provides (caching, parallelism).

### What's going on:

The `package.json` is very easy to understand. As always, `esy` looks for the
`"esy"` section and runs the build / install commands listed.

`esy` is not, and never will be, a build system - it merely is the glue to call
into your build system, and provides you critical information to forward to
your build scripts that make caching/reliability work. In this case, `cur__bin`
is the standard `esy` location where you install binary artifacts. This makes
sure that these artifacts will be seen by packages that depend on you in
*their* build scripts.


```
{
  "name": "esy-npm-project-example",
  "version": "1.0.0",
  "description": "Simple example of using esy on pure npm cli.",
  "scripts": {
    "install": "esybuild"
  },
  "esy": {
    "build": ["make install DESTINATION=$cur__bin"]
  },
  "dependencies": {
    "test": "file:../test",
    "esy": "https://github.com/reasonml/esy.git#beta-v-bleeding"
  }
}
```

### Typical use case:

More typically, you won't even acknowledge `cur__bin`. Instead you would use a
utility like `ocamlfind` or `opam-installer` to install packages and that
already knows where `cur__bin` is and so you won't have to talk about
`cur__bin`. This simple example just shows what's avaiable under the hood so
that you can integrate your *own* `ocamlfind` or equivalent.

