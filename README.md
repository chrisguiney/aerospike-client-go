# Aerospike Go Client [![Build Status](https://magnum.travis-ci.com/citrusleaf/go-client.svg?token=1yyz7BA7ChsFkpHDyZwW&branch=master)](https://magnum.travis-ci.com/citrusleaf/go-client)

An Aerospike library for Go.

This library is compatible with Go 1.2+ and supports the following operating systems: Linux, Mac OS X (Windows builds are possible, but untested)

- [Usage](#Usage)
- [Prerequisites](#Prerequisites)
- [Installation](#Installation)
- [Tests](#Tests)
- [Examples](#Examples)
- [Benchmarks](#Benchmarks)
- [API Documentaion](#API-Documentation)


## Usage:

The following is a very simple example of CRUD operations in an Aerospike database.

```go
package main

import (
  "fmt"

  . "github.com/citrusleaf/go-client"
)

func panicOnError(err error) {
  if err != nil {
    panic(err)
  }
}

func main() {
  // define a client to connect to
  client, err := NewClient("127.0.0.1", 3000)
  panicOnError(err)

  key, err := NewKey("test", "aerospike", "key")
  panicOnError(err)

  // define some bins with data
  bins := BinMap{
    "bin1": 42,
    "bin2": "An elephant is a mouse with an operating system",
    "bin3": []interface{}{"Go", 2009},
  }

  // write the bins
  err = client.Put(nil, key, bins)
  panicOnError(err)

  // read it back!
  rec, err := client.Get(nil, key)
  panicOnError(err)

  fmt.Printf("%#v\n", *rec)

  // delete the key, and check if key exists
  existed, err := client.Delete(nil, key)
  panicOnError(err)
  fmt.Printf("Record existed before delete? %v\n", existed)
}
```

More examples illustrating the use of the API are located in the
[`examples`](examples) directory.

Details about the API are available in the [`docs`](docs) directory.

<a name="Prerequisites"></a>
## Prerequisites

[Go](http://golang.org) version v1.2+ is required.
(It is possible to build the code in Go versions prior to 1.2, but our testing library depends on v1.2)

To install the latest stable version of Go, visit
[http://golang.org/dl/](http://golang.org/dl/)

Aerospike Go client implements the wire protocol, and does not depend on the C client.
It is goroutine friendly, and works asynchronously.

Supported operating systems:

- Major Linux distributions (Ubuntu, Debian, Redhat)
- Mac OS X
- Windows (untested)

<a name="Installation"></a>
## Installation:

1. Install Go 1.2+ and setup your environment as [Documented](http://golang.org/doc/code.html#GOPATH)
2. Get the client in your ```GOPATH``` : ```go get github.com/citrusleaf/go-client```
  * To update the client library: ```go get -u github.com/citrusleaf/go-client```

### Some Hints:

 * To run a go program directly: ```go run <filename.go>```
 * to build:  ```go build -o <output> <filename.go>```
  * example: ```go build -o benchmark tools/benchmark/benchmark.go```

<a name="Tests"></a>
## Tests

This library is packaged with a number of tests. Tests require Ginkgo and Gomega library.

Before running the tests, you need to update the dependencies:

    $ go get .

To run all the test cases with race detection:

    $ ginkgo -r -race


<a name="Examples"></a>
## Examples

A variety of example applications are provided in the [`examples`](examples) directory.
See the [`examples/README.md`](examples/README.md) for details.

<a name="Benchmarks"></a>
## Benchmarks

Benchmark utility is provided in the [`tools/benchmark`](tools/benchmark) directory.
See the [`tools/benchmark/README.md`](tools/benchmark/README.md) for details.

<a name="API-Documentation"></a>
## API Documentation

API documentation is available using godocs.

A preformatted version is provided in the [`docs`](docs/README.md) directory.

## License

The Aerospike Go Client is made availabled under the terms of the Apache License, Version 2, as stated in the file `LICENSE`.

Individual files may be made available under their own specific license,
all compatible with Apache License, Version 2. Please see individual files for details.
