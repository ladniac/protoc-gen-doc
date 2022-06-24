# protoc-gen-doc

This is a fork of documentation generator plugin for the Google Protocol Buffers compiler (`protoc`). It has been modified to help generate mqtt/protobuf API documentation. The plugin can generate
HTML documentation from comments in your `.proto` files.

It supports proto2 and proto3, and can handle having both in the same context (see [examples](examples/) for proof).

## Installation

```sh
    git clone git@github.com:ladniac/protoc-gen-doc.git
    cd protoc-gen-doc
    make build
```

### Simple Usage

```sh
    export PROTOC_GEN_DOC_RESOURCES_PATH=path/to/documentation/folder
```
documentation folder should contain:
conf.yaml - configuration file
overview.html - Overview of the project
stylesheet.css - custom styles
see examples in examples/doc_folder_example

For example, to generate HTML documentation for file.proto into `doc/index.html`, type:
```sh
    cd
    protoc --doc_out=./docs --doc_opt=html,index.html --plugin=bin/protoc-gen-doc file.proto
```

## Writing Documentation

Messages, Fields, Services (and their methods), Enums (and their values), Extensions, and Files can be documented.
Generally speaking, comments come in 2 forms: leading and trailing.

**Leading comments**

Leading comments can be used everywhere.

```protobuf
/**
 * This is a leading comment for a message
 */
message SomeMessage {
  // this is another leading comment
  string value = 1;
}
```

> NOTE: File level comments should be leading comments on the syntax directive.

**Trailing comments**

Fields, Service Methods, Enum Values and Extensions support trailing comments.

```protobuf
enum MyEnum {
  DEFAULT = 0; // the default value
  OTHER   = 1; // the other value
}
```

**Excluding comments**

If you want to have some comment in your proto files, but don't want them to be part of the docs, you can simply prefix
the comment with `@exclude`. 

Example: include only the comment for the `id` field

```protobuf
/**
 * @exclude
 * This comment won't be rendered
 */
message ExcludedMessage {
  string id   = 1; // the id of this message.
  string name = 2; // @exclude the name of this message

  /* @exclude the value of this message. */
  int32 value = 3;
}
```

To mark a message as a mqtt topic:

```protobuf
/**
 * <span class='topic publish'>PUBLISH topic/a/b</span>
 * This is a leading comment for a message
 */
message SomeMessage {
  // this is another leading comment
  string value = 1;
}
```
