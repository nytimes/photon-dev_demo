# Photon Demo, Part 1

Hello! 👋

This demo supplements the "Sustainable Systems, Powered By Python" talk presented by Michael
Laing and Sharon Tartarone at All Things Open 2019, and illustrates key concepts, including:

* the "MONTY" repository structure
* installation and configurable runs (setuptools and click)
* code quality and type checking (black, flake8, and mypy)
* feature addition (pluggy)
* resilient runtime (failfast and startfast)
* automated documentation (sphinx)

The complete demo consists of the following repositories:

* Photon Development ([nytimes/photon-dev_demo](https://github.com/nytimes/photon-dev_demo))
* Photon Application ([nytimes/photon-app_demo](https://github.com/nytimes/photon-app_demo))
* Photon Common ([nytimes/photon-common_demo](https://github.com/nytimes/photon-common_demo))

If you want to get started right away, jump to the [Setup](#Setup) section. Otherwise, peruse the details below for more detailed information.

# Photon Development

Photon Development provides the parent project for the local development, documentation and testing of the Photon family of applications.

## Features
The Photon Development project includes:

* A shell script (`bin/photon`) for local development & testing of packages; and
* Sphinx documentation builder for Photon family projects.

## Details
Photon family projects (`nytimes/photon-*`) may be cloned into this project, e.g. `nytm/photon-app_demo`. If you want to maintain common code then clone `nytm/photon-common_demo`.

This Photon repo includes the `photon` shell script which may be copied into `/usr/local/bin`. When 'sourced' (`source photon`) it will create a shared virtual environment in 'develop' mode, i.e. `pip install --editable ... -r requirements.txt -r requirements_dev.txt`, for all the `photon-*` subprojects based in the `photon-dev_demo` directory including `photon-common_demo`. It will also install the requirements for building docs.

The `docs/` directory has the objects necessary to do sphinx build of the overview docs and the docs for subprojects. Note that this is done via linking in `index.rst` and folder links to the `docs/` directories in the subprojects. All subprojects should follow this pattern.

## Setup

### Prerequisites

TBD

### Installation

1. Clone this repository (this will be the parent directory).
2. Copy `bin/photon` into `/usr/local/bin`.
2. Clone Photon family projects into this project (as subdirectories).
3. Within any part of the "MONTY" structure, run `source photon` to activate the venv.

## Generating Docs

1. `cd docs/` then `make html` will build the html version
2. `open _build/html/index.html` will display it in your default browser
