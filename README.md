# uint8arraylist <!-- omit in toc -->

[![codecov](https://img.shields.io/codecov/c/github/achingbrain/uint8arraylist.svg?style=flat-square)](https://codecov.io/gh/achingbrain/uint8arraylist)
[![CI](https://img.shields.io/github/workflow/status/achingbrain/uint8arraylist/test%20&%20maybe%20release/master?style=flat-square)](https://github.com/achingbrain/uint8arraylist/actions/workflows/js-test-and-release.yml)

> Append and consume bytes using only no-copy operations

## Table of contents <!-- omit in toc -->

- [Install](#install)
- [Usage](#usage)
  - [Converting Uint8ArrayLists to Uint8Arrays](#converting-uint8arraylists-to-uint8arrays)
    - [slice](#slice)
    - [subarray](#subarray)
    - [sublist](#sublist)
- [Inspiration](#inspiration)
- [License](#license)
- [Contribution](#contribution)

## Install

```console
$ npm i uint8arraylist
```

## Usage

```js
import { Uint8ArrayList } from 'uint8arraylist'

const list = new Uint8ArrayList()
list.append(Uint8Array.from([0, 1, 2]))
list.append(Uint8Array.from([3, 4, 5]))

list.subarray()
// -> Uint8Array([0, 1, 2, 3, 4, 5])

list.consume(3)
list.subarray()
// -> Uint8Array([3, 4, 5])

// you can also iterate over the list
for (const buf of list) {
  // ..do something with `buf`
}

list.subarray(0, 1)
// -> Uint8Array([0])
```

### Converting Uint8ArrayLists to Uint8Arrays

There are two ways to turn a `Uint8ArrayList` into a `Uint8Array` - `.slice` and `.subarray` and one way to turn a `Uint8ArrayList` into a `Uint8ArrayList` with different contents - `.sublist`.

#### slice

Slice follows the same semantics as [Uint8Array.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray/slice) in that it creates a new `Uint8Array` and copies bytes into it using an optional offset & length.

```js
const list = new Uint8ArrayList()
list.append(Uint8Array.from([0, 1, 2]))
list.append(Uint8Array.from([3, 4, 5]))

list.slice(0, 1)
// -> Uint8Array([0])
```

#### subarray

Subarray attempts to follow the same semantics as [Uint8Array.subarray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray/subarray) with one important different - this is a no-copy operation, unless the requested bytes span two internal buffers in which case it is a copy operation.

```js
const list = new Uint8ArrayList()
list.append(Uint8Array.from([0, 1, 2]))
list.append(Uint8Array.from([3, 4, 5]))

list.slice(0, 1)
// -> Uint8Array([0]) - no-copy

list.slice(2, 5)
// -> Uint8Array([2, 3]) - copy
```

#### sublist

Sublist creates and returns a new `Uint8ArrayList` that shares the underlying buffers with the original so is always a no-copy operation.

```js
const list = new Uint8ArrayList()
list.append(Uint8Array.from([0, 1, 2]))
list.append(Uint8Array.from([3, 4, 5]))

list.sublist(0, 1)
// -> Uint8ArrayList([0]) - no-copy

list.sublist(2, 5)
// -> Uint8ArrayList([2, 3]) - no-copy
```

## Inspiration

Borrows liberally from [bl](https://www.npmjs.com/package/bl) but only uses native JS types.

## License

Licensed under either of

- Apache 2.0, ([LICENSE-APACHE](LICENSE-APACHE) / <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT ([LICENSE-MIT](LICENSE-MIT) / <http://opensource.org/licenses/MIT>)

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
