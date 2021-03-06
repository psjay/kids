# kids

[![Build Status]][Travis CI]

Kids is a log aggregation system.

It aggregates messages like [Scribe](https://github.com/facebookarchive/scribe) and its pub/sub pattern is ported from [Redis](http://redis.io/).


## Features

* Real-time subscription
* Distributed collection
* Message persistence
* Multithreading
* Redis protocol
* No third-party dependencies


## Installation

You need a complier with C++11 support like GCC 4.7 (or later) or [Clang](http://clang.llvm.org).

Download a [release](https://github.com/zhihu/kids/releases). Untar the tarball, then:

    ./configure
    make
    make test  # optional
    make install

By default, it will be installed to `/usr/local/bin/kids`.
You can use the `--prefix` option to specify the installation location.
Run `./configure --help` for more config options.


## Quickstart

Kids comes with some sample config files in `samples/`, after building, simply run:

    kids -c samples/dev.conf

Because kids uses redis protocol, so you can use `redis-cli` to play with it, open another terminal:
    
    $ redis-cli -p 3888
    $ 127.0.0.1:3388> PSUBSCRIBE *

In yet another terminal:
    
    $ redis-cli -p 3388
    $ 127.0.0.1:3388> PUBLISH test message

`redis-cli` needs `redis` to be installed, in MAC, you can run `brew install redis` to install it.

Full explanation of config file, see [here](doc/config.md).

Run `kids --help` for more running options.

## Run in production

In production, we deploy kids agent at every host, and assign a powerful server to kids server,

We now support making deb package to simplify deployment, to do this, you need:

* build-essential, libtool, automake for building the prject
* [fpm](https://github.com/jordansissel/fpm) for packaging

### Steps

    git clone https://github.com/zhihu/kids.git
    cd kids
    ./autogen.sh
    ./configure
    make
    cp samples/agent.conf debian/
    # EDIT the config file to fit into your environment: for agent, if you use samples/agent.conf then
    # you only need to fill in kids server's hostname
    cd debian
    ./make_deb.sh

For server, use the same deb package and overwrite /etc/kids.conf with server's config file.

## License

Kids is BSD-licensed, see LICENSE for more details.


## FAQ

Q: What is the meaning of "kids"?  
A: "kids" is the recursive acronym of "Kids Is Data Stream".


## Architecture

![image](doc/image/arch.jpg)

You can view the Chinese version README [here](README.zh_CN.md)


[Build Status]: https://img.shields.io/travis/zhihu/kids/master.svg?style=flat
[Travis CI]:    https://travis-ci.org/zhihu/kids
