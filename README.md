# go-libp2p-gostream

[![Build Status](https://travis-ci.org/hsanjuan/go-libp2p-gostream.svg?branch=master)](https://travis-ci.org/hsanjuan/go-libp2p-gostream)
[![Coverage Status](https://coveralls.io/repos/github/hsanjuan/go-libp2p-gostream/badge.svg?branch=master)](https://coveralls.io/github/hsanjuan/go-libp2p-gostream?branch=master)
[![standard-readme compliant](https://img.shields.io/badge/standard--readme-OK-green.svg)](https://github.com/RichardLitt/standard-readme)


> Go "net" wrappers for LibP2P

Package `gostream` allows to replace the standard net stack in Go with [LibP2P](https://github.com/libp2p/libp2p) streams.

Given a libp2p.Host, `gostream` provides Dial() and Listen() methods which return implementations of net.Conn and net.Listener.

Instead of the regular "host:port" addressing, `gostream` uses a Peer ID, and rather than a raw TCP connection, gostream will use libp2p's net.Stream. This means your connections will take advantage of  LibP2P's multi-routes, NAT transversal and stream multiplexing.

## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [Contribute](#contribute)
- [License](#license)

## Install

This package uses [`gx`](https://github.com/whyrusleeping/gx-go) for dependencies and should be imported with `gx` on other projects:

```
$ gx import github.com/hsanjuan/go-libp2p-gostream
```

The code can be downloaded and tested with:

```
$ go get -u -d github.com/hsanjuan/go-libp2p-gostream
$ cd $GOPATH/src/github.com/hsanjuan/go-libp2p-gostream
$ make test
```

## Usage

Documentation can be read at [Godoc](https://godoc.org/github.com/hsanjuan/go-libp2p-gostream). The important bits follow.

A simple communication between peers -one acting as server and the other as client- would work like:

```go
	go func() {
		listener, _ := Listen(srvHost, tag)
		defer listener.Close()
		servConn, _ := listener.Accept()
		defer servConn.Close()
		reader := bufio.NewReader(servConn)
		msg, _ := reader.ReadString('\n')
        fmt.Println(msg)
		servConn.Write([]byte("answer!\n"))
	}()

	clientConn, _ := Dial(clientHost, srvHost.ID(), tag)
    clientConn.Write([]byte("question?\n"))
	resp, _ := ioutil.ReadAll(clientConn)
    fmt.Println(resp)
```

Note error handling above is ommited.

## Contribute

PRs accepted.

## License

MIT © Hector Sanjuan
