# PyCoral API

This repository contains an easy-to-use Python API that helps you run inferences
and perform on-device transfer learning with TensorFlow Lite models on
[Coral devices](https://coral.ai/products/).

To install the prebuilt PyCoral library, see the instructions at
[coral.ai/software/](https://coral.ai/software/#pycoral-api).

**Note:** If you're on a Debian system, be sure to install this library from
apt-get and not from pip. Using `pip install` is not guaranteed compatible with
the other Coral libraries that you must install from apt-get. For details, see
[coral.ai/software/](https://coral.ai/software/#debian-packages).

## Documentation and examples

To learn more about how to use the PyCoral API, see our guide to [Run inference
on the Edge TPU with Python](https://coral.ai/docs/edgetpu/tflite-python/) and
check out the [PyCoral API reference](https://coral.ai/docs/reference/py/).

Several Python examples are available in the `examples/` directory. For
instructions, see the [examples README](
https://github.com/google-coral/pycoral/tree/master/examples#pycoral-api-examples).


## Compilation

When building this library yourself, it's critical that you have
version-matching builds of
[libcoral](https://github.com/google-coral/libcoral/tree/master) and
[libedgetpu](https://github.com/google-coral/libedgetpu/tree/master)—notice
these are submodules of the pycoral repo, and they all share the same
`TENSORFLOW_COMMIT` value. So just be sure if you change one, you must change
them all.

For complete details about how to build all these libraries, read
[Build Coral for your platform](https://coral.ai/docs/notes/build-coral/).
Or to build just this library, follow these steps:

1.  Clone this repo and include submodules:

    ```
    git clone --recurse-submodules https://github.com/feranick/pycoral
    ```

    If you already cloned without the submodules. You can add them with this:

    ```
    cd pycoral

    git submodule init && git submodule update
    ```

## Compilation using Docker:

* Edit the following lines in `scripts/build.sh` to reflect your compilation targets:

```
readonly DOCKER_CPUS="${DOCKER_CPUS:=k8 aarch64 armv7a}"
PYTHON_VERSIONS="3.9 3.10 3.11 3.12"
```

For example if compiling for only `x86_64` and `python 3.10`, the above line should be:
```
readonly DOCKER_CPUS="${DOCKER_CPUS:=k8}"
PYTHON_VERSIONS="3.10"
```
  
For multiple architectures:

```
readonly DOCKER_CPUS="${DOCKER_CPUS:=k8 aarch64}"
PYTHON_VERSIONS="3.10"
```

For multiple versions of phyton:

```
readonly DOCKER_CPUS="${DOCKER_CPUS:=k8}"
PYTHON_VERSIONS="3.10 3.12"
```

* Run `scripts/build.sh` to build pybind11-based native layer for different
    Linux architectures. Build is Docker-based, so you need to have it
    installed.

* Run `make wheel` to generate Python library wheel and then `pip3 install
    $(ls dist/*.whl)` to install it.
    
## Native Compilation:

`bazel` is required for native compilation. The correct and required version of `bazel` for the specific version of TensorFlow is listed here:

https://www.tensorflow.org/install/source

Run the following to generate Python library wheel:

```
TF_PYTHON_VERSION=3.xx make pybind
TF_PYTHON_VERSION=3.xx make wheel
```

where `3.xx` refers to the version of Python you are compiling against (3.10, 3.11, 3.12, etc). Then run:

```
pip3 install
$(ls dist/*.whl)
```

to install it.
