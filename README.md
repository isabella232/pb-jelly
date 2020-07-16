# pb-rs

`pb-rs` is a protobuf code generation framework for the rust language developed at Dropbox.

# Features
- Scalable - generates separate crates per module, with option for crate-per-directory
- Closed structs with public fields
- Autogenerates Cargo.toml, or optionally Spec.toml / bazel BUILD files
- Supports generating non-nullable fields types with `(gogoproto.nullable)=false`
- Supports generating `Box<Message>` field types with `(rust.box_it)=true`
- Automatically boxes messages if it finds a recursive message definition
- Supports generating custom field type with `(rust.custom_type)=type`
- Supports generating oneofs as non-nullable (fail on deserialization) type with `(rust.nullable)=false`
- Supports generating enums as non-zeroable (fail on deserialization) type with `(rust.err_if_default_or_unknown)=true`
- Supports generating serde serializable/deserializable messages with file level `(rust.serde_derive)=true`
- Supports preserving unrecognized proto fields into `_unrecognized` struct field with `(rust.preserve_unrecognized)=true`
  - Defaults to false - to eliminate serde dependency
- zero-copy deserialization (coming soon)
- service generation (coming soon)

# Shortcomings
Some of the features here require additional tooling to be useful, which is not public yet.
- Spec.toml is a stripped down templated Cargo.toml - which you can script convert into
    Cargo.toml in order to get consistent dependency versions in a multi-crate project.
    Currently, the script to convert Spec.toml -> Cargo.toml isn't yet available
- Autogenerated BUILD files require additional tooling to convert `BUILD.in-gen-proto~` to a BUILD file

Closed structs with public fields
- Adding fields to a proto file will lead to compiler errors. This can be a benefit in that it allows the
compiler to identify all callsites that may need to be visited. However, it can make updating protos with
many callsites a bit tedious. We opted to go this route to make it easier to add a new field and update
all callsites with assistance from the compiler.

# Contributing

Outside contributors must agree to Dropbox's CLA https://opensource.dropbox.com/cla/

# Running the `pbtest` unit tests

1. Clone Repo
2. Install Dependencies / Testing Dependencies
	a. protoc
		- On OSX: `brew install protobuf`
	b. Install go
		- go: `brew install go`
	b. Install gogoproto
		- `go get github.com/gogo/protobuf/proto`
	c. Install python & dependencies
		- `brew install python3`
		- `pip install six`
		- `pip install protobuf`
    d. Generate test protos
        - On OSX: you'll have to install coreutils for realpath `brew install coreutils`
        - `./gen_protos.sh
3. `cargo test`


### TODO - before open sourcing

- [x] Get onto github
- [x] Get lib compiling
- [x] Get tests compiling
- [x] Add critical protos (eg rust.proto for extensions)
- [x] Move pbtest into tests/ directory per traditional rust testing design
- [x] Add pbtest protos
- [x] Get protos compiling somehow for tests
- [x] Document features of this implementation
- [x] Add open source license (Apache 2.0) and Copyright attribution `"Copyright (c) 2020 Dropbox, Inc."` - http://www.apache.org/licenses/LICENSE-2.0
- [x] Credit other Dropboxers that have contributed to pb-rs development (look at git log)
- [x] Link to CLA for outside contributions https://opensource.dropbox.com/cla/
- [ ] Document why it exists (vs the standard open source proto options)
- [ ] Make extensions.proto and servicepb.proto proto3
- [ ] Add examples to README
- [ ] remove this section from the README
- [ ] Remove references to dbx specific stuff
- [ ] Add requirements.txt for python dependencies

### TODO

- [ ] Document and add tests for `grpc_slices`
- [ ] Add `custom_type` example to pbtest
- [ ] Mypy the codegen.py in CI
- [ ] Add service generation codegen as well
- [ ] Rename pbtest.proto to pbtest2.proto
- [ ] Add blob crate / zerocopy
- [ ] run benchmarks against zerocopy blob impl
- [ ] Create github issues for remaining todos
- [ ] Add travis-ci integration
- [ ] Run mypy-protobuf for the tests - to better mypy codegen.py
- [ ] Autogenerate warn on rust 2018 idioms
- [ ] Add test files w/ multiple generated crates that depend on each other
- [ ] Figure out how to host documentation somewhere
- [ ] Bonus: Port over the test which serializes in go and deserializes/reserializes in rust
- [ ] Bonus: Python3
- [ ] Create helper library similar to `prost_build`
- [ ] Open source oxidize (or something of the sort)

## Contributors

### Dropboxers
- [@nipunn1313](https://github.com/nipunn1313)
- [@rajatgoel](https://github.com/rajatgoel)
- [@rbtying](https://github.com/rbtying)
- [@goffrie](https://github.com/goffrie)
- [@euroelessar](https://github.com/euroelessar)
- [@bradenaw](https://github.com/bradenaw)
- [@aaronp]
- [@jiayixu](https://github.com/jiayixu)
- [@dyv](https://github.com/dyv)
- [@joshuawarner32](https://github.com/joshuawarner32)
- [@peterlvilim](https://github.com/peterlvilim)
- [@ddeville](https://github.com/ddeville)
- [@isho](https://github.com/isho)
