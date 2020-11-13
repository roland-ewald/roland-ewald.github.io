---
title:  "Compiling plv8 extension for PostgreSQL 12"
date:   2020-11-13
tags: postgresql plv8 ubuntu js
---

There are no Linux binaries available for recent versions of the [plv8](https://github.com/plv8/plv8) extesion, which adds JavaScript support to PostreSQL. 
Compiling the extension manually is a little tricky, because additional dependencies are necessary (discussed [here](https://github.com/plv8/plv8/issues/283#issuecomment-397359439)), while no copy of the V8 library, such as `libnode-dev`, must be installed (discussed [here](https://github.com/plv8/plv8/issues/387#issuecomment-605663975)). The build also relies on [python 2](https://xkcd.com/353/). This is how it should work on a Ubuntu 20.10 setup:

```bash
# See https://github.com/plv8/plv8/issues/387#issuecomment-605663975 for all libraries that may be an issue:
sudo apt-get remove libnode libnode-dev
# See https://github.com/plv8/plv8/issues/283#issuecomment-397359439
sudo apt-get install python postgresql-server-dev-12 make git pkg-config chromium-browser subversion clang apg ninja-build cmake libc++-dev libc++abi-dev
wget https://github.com/plv8/plv8/archive/v2.3.15.tar.gz
tar xfvz v2.3.15.tar.gz
cd plv8...
sudo make
sudo make install
make installcheck
```

Enjoy!