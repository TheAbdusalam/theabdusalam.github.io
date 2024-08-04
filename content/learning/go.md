---
title: "Learning Go"
description: "Some notes on learning Go"
date: "2024-08-04"
tags:
- Go
- Language
- Notes
---

already familiar with Go, but there is always more to learn. This is a collection of notes on learning Go.

## Table of Contents
- 0x0000: [Channeling](#channeling)
- 0xx0001: [Concurrency](#concurrency)
- 0x0002: [Error Handling](#error-handling)
- 0x0003: [Interfaces](#interfaces)
- 0x0004: [Packages](#packages)
- 0x0005: [Pointers](#pointers)
- 0x0006: [Slices](#slices)

## Channeling
Channels are a way to communicate between goroutines. They are typed, so you can only send and receive the type that the channel is defined with. Channels can be buffered or unbuffered. Unbuffered channels block until the data is received. Buffered channels can hold a certain number of values before blocking.

```go
package main

import "fmt"

func main() {
    ch := make(chan int, 1)
    ch <- 42
    fmt.Println(<-ch)
}
```
