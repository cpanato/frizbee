![image](https://github.com/stacklok/frizbee/assets/16540482/35034046-d962-475d-b8e2-67b7625f2a60)

---
[![Coverage Status](https://coveralls.io/repos/github/stacklok/frizbee/badge.svg?branch=main)](https://coveralls.io/github/stacklok/frizbee?branch=main) | [![License: Apache 2.0](https://img.shields.io/badge/License-Apache2.0-brightgreen.svg)](https://opensource.org/licenses/Apache-2.0) | [![](https://dcbadge.vercel.app/api/server/RkzVuTp3WK?logo=discord&label=Discord&color=5865&style=flat)](https://discord.gg/RkzVuTp3WK)

---
# Frizbee

Frizbee is a tool you may throw a tag at and it comes back with a checksum.

It's a command-line tool designed to provide checksums for GitHub Actions
and container images based on tags.

It also includes a set of libraries for working with tags and checksums.

## Table of Contents

- [Installation](#installation)
- [Usage - CLI](#usage---cli)
  - [GitHub Actions](#github-actions)
  - [Container Images](#container-images)
- [Usage - Library](#usage---library)
  - [GitHub Actions](#github-actions)
  - [Container Images](#container-images)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)

## Installation

To install Frizbee, you can use the following methods:

```bash
# Using Go
go get -u github.com/stacklok/frizbee
go install github.com/stacklok/frizbee

# Using Homebrew
brew install stacklok/tap/frizbee

# Using winget
winget install stacklok.frizbee
```

## Usage - CLI

### GitHub Actions

Frizbee can be used to generate checksums for GitHub Actions. This is useful
for verifying that the contents of a GitHub Action have not changed.

To quickly replace the GitHub Action references for your project, you can use
the `actions` command:

```bash
frizbee actions path/to/your/repo/.github/workflows/
```

This will write all the replacements to the files in the directory provided.

Note that this command will only replace the `uses` field of the GitHub Action
references.

Note that this command supports dry-run mode, which will print the replacements
to stdout instead of writing them to the files.

It also supports exiting with a non-zero exit code if any replacements are found. 
This is handy for CI/CD pipelines.

If you want to generate the replacement for a single GitHub Action, you can use the
same command:

```bash
frizbee actions metal-toolbox/container-push/.github/workflows/container-push.yml@main
```

This is useful if you're developing and want to quickly test the replacement.

### Container Images

Frizbee can be used to generate checksums for container images. This is useful
for verifying that the contents of a container image have not changed. This works
for all yaml/yml and Dockerfile fies in the directory provided by the `-d` flag.

To quickly replace the container image references for your project, you can use
the `image` command:

```bash
frizbee image path/to/your/yaml/files/
```

To get the digest for a single image tag, you can use the same command:

```bash
frizbee image ghcr.io/stacklok/minder/server:latest
```

This will print the image reference with the digest for the image tag provided.

## Usage - Library

Frizbee can also be used as a library. The library provides a set of functions
for working with tags and checksums. Here are a few examples of how you can use
the library:

### GitHub Actions

```go
// Create a new replacer
r := replacer.NewGitHubActionsReplacer(cfg)
...
// Parse a single GitHub Action reference
ret, err := r.ParseString(ctx, ghActionRef)
...
// Parse all GitHub Actions workflow yaml files in a given directory
res, err := r.ParsePath(ctx, dir)
...
// Parse and replace all GitHub Actions references in the provided file system
res, err := r.ParsePathInFS(ctx, bfs, base)
...
// Parse a single yaml file referencing GitHub Actions
res, err := r.ParseFile(ctx, fileHandler)
...
// List all GitHub Actions referenced in the given directory
res, err := r.ListPath(dir)
...
// List all GitHub Actions referenced in the provided file system
res, err := r.ListPathInFS(bfs, base)
...
// List all GitHub Actions referenced in the provided file
res, err := r.ListFile(fileHandler)
```

### Container images 

```go
// Create a new replacer
r := replacer.NewContainerImagesReplacer(cfg)
...
// Parse a single container image reference
ret, err := r.ParseString(ctx, ghActionRef)
...
// Parse all files containing container image references in a given directory
res, err := r.ParsePath(ctx, dir)
...
// Parse and replace all container image references in the provided file system
res, err := r.ParsePathInFS(ctx, bfs, base)
...
// Parse a single yaml file referencing container images
res, err := r.ParseFile(ctx, fileHandler)
...
// List all container images referenced in the given directory
res, err := r.ListPath(dir)
...
// List all container images referenced in the provided file system
res, err := r.ListPathInFS(bfs, base)
...
// List all container images referenced in the provided file
res, err := r.ListFile(fileHandler)
```

## Configuration

Frizbee can be configured by setting up a `.frizbee.yml` file. 
You can configure Frizbee to skip processing certain actions, i.e.

```yml
ghactions:
  exclude:
    # Exclude the SLSA GitHub Generator workflow.
    # See https://github.com/slsa-framework/slsa-github-generator/issues/2993
    - slsa-framework/slsa-github-generator/.github/workflows/generator_generic_slsa3.yml

```

## Contributing

We welcome contributions to Frizbee. Please see our [Contributing](./CONTRIBUTING.md) guide for more information.

## License

Frizbee is licensed under the [Apache 2.0 License](./LICENSE).
