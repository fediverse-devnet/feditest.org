# FediTest.org

This is the content of [feditest.org](https://feditest.org/).

It is built with the [Hugo](https://gohugo.io/) static site generator, plus some Python scripts that generate a few of the pages.

## Software requirements

Install Git and Git-LFS, and initialize it:

```shell
$ brew install git
$ brew install git-lfs
$ git lfs install
```

Install Hugo, e.g. on the Mac:

```shell
$ brew install hugo
```

## To run the website locally

Get the content:

```shell
$ git clone https://github.com/fediverse-devnet/feditest.org.git
$ cd feditest.org
$ git submodule update
```

Run:

```shell
$ hugo server
```

Then go to `http://localhost:1313/` (actual port will be printed to the terminal).
