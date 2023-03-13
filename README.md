# did

[![GoDoc](https://img.shields.io/badge/godoc-reference-blue.svg?style=flat-square)](https://godoc.org/github.com/nuts-foundation/did-ockam)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=flat-square)](LICENSE)

`did` is a Go package that provides tools to work with
[Decentralized Identifiers (DIDs)](https://w3c-ccg.github.io/did-spec).

This repository is a fork of [ockam-network/did](https://github.com/build-trust/did).

## Install

```
go get github.com/nuts-foundation/did-ockam
```

## Example

```go
package main

import (
	"fmt"
	"log"

	"github.com/nuts-foundation/did-ockam"
)

func main() {
	d, err := did.Parse("did:example:q7ckgxeq1lxmra0r")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%#v", d)
}
```

The above example parses the input string according to the rules defined in the [DID Grammar](did.abnf) and prints the
following value of DID type.

```go
&did.DID{
	Method:"example",
	ID:"q7ckgxeq1lxmra0r",
	IDStrings:[]string{"q7ckgxeq1lxmra0r"},
	Path:"",
	PathSegments:[]string(nil),
	Query:"",
	Fragment:""
}
```

The input string may also be a [DID Reference](https://w3c-ccg.github.io/did-spec/#dfn-did-reference) with a
[DID Path](https://w3c-ccg.github.io/did-spec/#dfn-did-path):

```go
d, err := did.Parse("did:example:q7ckgxeq1lxmra0r/abc/pqr")
```

which would result in:

```go
&did.DID{
	Method:"example",
	ID:"q7ckgxeq1lxmra0r",
	IDStrings:[]string{"q7ckgxeq1lxmra0r"},
	Path:"abc/pqr",
	PathSegments:[]string{"abc", "pqr"},
	Query:"",
	Fragment:""
}
```

or a [DID Reference](https://w3c-ccg.github.io/did-spec/#dfn-did-reference) with a
[DID Path](https://w3c-ccg.github.io/did-spec/#dfn-did-path) and a DID Query:

```go
d, err := did.Parse("did:example:q7ckgxeq1lxmra0r/abc/pqr?xyz")
fmt.Println(d.Query)
// Output: xyz
```

or a [DID Reference](https://w3c-ccg.github.io/did-spec/#dfn-did-reference) with a
[DID Fragment](https://w3c-ccg.github.io/did-spec/#dfn-did-fragment):

```go
d, err := did.Parse("did:example:q7ckgxeq1lxmra0r#keys-1")
fmt.Println(d.Fragment)
// Output: keys-1
```

This package also implements the [Stringer](https://golang.org/pkg/fmt/#Stringer) interface for the DID type. It is
easy to convert DID type structures into valid DID strings:

```go
d := &did.DID{Method: "example", ID: "q7ckgxeq1lxmra0r"}
fmt.Println(d.String())
// Output: did:example:q7ckgxeq1lxmra0r
```

or with a refence with a fragment:

```go
d := &did.DID{Method: "example", ID: "q7ckgxeq1lxmra0r", Fragment: "keys-1"}
fmt.Println(d.String())
// Output: did:example:q7ckgxeq1lxmra0r#keys-1
```

For more documentation and examples, please see [godoc](https://godoc.org/github.com/nuts-foundation/did-ockam).

## Build

To compile the code in this repository, run:

```
go build
```

## Test

This repository includes a comprehensive [suite of tests](did_test.go) that check for various edge cases within
the [DID Grammar](did.abnf).

To run the tests, run:

```
go test -v -cover
```

## Benchmark

We haven't spent any time tuning the performance of the parser, however this repository includes some
[benchmarks](benchmark_test.go) that compare the speed of `did.Parse` against Go's `url.Parse` with inputs
of similar length, this is intended as a sanity check to ensure that `did.Parse` is at least comparable in performance
to `url.Parse`

```
go test -bench=.
```

`did.Parse` included in this package:

```
BenchmarkParse-8                  	 5000000	       365 ns/op
BenchmarkParseWithPath-8          	 3000000	       500 ns/op
BenchmarkParseWithQuery-8         	 3000000	       558 ns/op
BenchmarkParseWithFragment-8      	 3000000	       552 ns/op
```

Go's `url.Parse`:

```
BenchmarkUrlParse-8               	 3000000	       475 ns/op
BenchmarkUrlParseWithPath-8       	 3000000	       505 ns/op
BenchmarkUrlParseWithQuery-8      	 5000000	       294 ns/op
BenchmarkUrlParseWithFragment-8   	 5000000	       369 ns/op
```

## Contributing

This package is early in its development and we welcome all contributions from the DID community. Please open issues
and send pull requests.

We follow the conventions specified in [Conventional Commits](https://www.conventionalcommits.org/) for our commit
messages.

## License

This package is licensed under [Apache License 2.0](LICENSE).
