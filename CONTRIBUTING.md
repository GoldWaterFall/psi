# Contribution guidelines

## Contributor License Agreement

Contributions to this project must be accompanied by a Contributor License
Agreement. You (or your employer) retain the copyright to your contribution;
this simply gives us permission to use and redistribute your contributions as
part of the project.

## Style

### C++ coding style

In general, please use clang-format to format code, and follow clang-tidy tips.

Most of the code style is derived from the
[Google C++ style guidelines](https://google.github.io/styleguide/cppguide.html), except:

- Exceptions are allowed and encouraged where appropriate.
- Header guards should use `#pragma once`.
- Adopt [camelBack](https://llvm.org/docs/Proposals/VariableNames.html#variable-names-coding-standard-options)
    for function names.
- Use [fixed width integer types](https://en.cppreference.com/w/cpp/types/integer) whenever possible.
- Avoid using size_t on interface APIs.

The compiler portion of the project follows [MLIR style](https://mlir.llvm.org/getting_started/DeveloperGuide/#style-guide).

### Other tips

- Git commit message should be meaningful, we suggest imperative [keywords](https://github.com/joelparkerhenderson/git_commit_message#summary-keywords).
- Developer must write unit-test (line coverage must be greater than 80%), tests should be deterministic.
- Read awesome [Abseil Tips](https://abseil.io/tips/)

## Build

### Prerequisite


#### Docker

```sh
## start container
docker run -d -it --name psi-dev-$(whoami) \
         --mount type=bind,source="$(pwd)",target=/home/admin/dev/ \
         -w /home/admin/dev \
         --cap-add=SYS_PTRACE --security-opt seccomp=unconfined \
         --cap-add=NET_ADMIN \
         --privileged=true \
         secretflow/ubuntu-base-ci:latest \
         bash

# attach to build container
docker exec -it psi-dev-$(whoami) bash
```

#### Linux

```sh
Install gcc>=11.2, cmake>=3.26, ninja, nasm>=2.15, python>=3.10, bazelisk, xxd, lld
```

#### macOS

```sh
# macOS >= 13.0, Xcode >= 15.0

# Install Xcode
https://apps.apple.com/us/app/xcode/id497799835?mt=12

# Select Xcode toolchain version
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer

# Install homebrew
https://brew.sh/

# Install dependencies
# Be aware, brew may install a newer version of bazel, when that happens bazel will give an error message during build.
# Please follow instructions in the error message to install the required version
brew install bazelisk cmake ninja libomp wget

# For Intel mac only
brew install nasm
```

### Build & UnitTest




``` sh
# build as debug
bazel build //... -c dbg

# build as release
bazel build //... -c opt

# test
bazel test //...

# [optional] build & test with ASAN or UBSAN, for macOS users please use configs with macOS prefix
bazel test //... --features=asan
bazel test //... --features=ubsan
```

### Bazel build options

- `--define gperf=on` enable gperf

### Build docs

```sh
# prerequisite
pip install -U -r docs/requirements.txt

cd docs && make html  # html docs will be in docs/_build/html
```
