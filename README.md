# Photon Development Demo

Hello! 👋

This demo supplements the "Sustainable Systems, Powered By Python" talk presented by Michael Laing and Sharon Tartarone at All Things Open 2019 and illustrates key concepts, including:

* the "MONTY" repository structure
* installation and configurable runs ([setuptools](https://setuptools.readthedocs.io) and [click](https://click.palletsprojects.com))
* code quality and type checking ([black](https://black.readthedocs.io/en/stable/), [flake8](http://flake8.pycqa.org), and [mypy](https://mypy.readthedocs.io))
* feature addition ([pluggy](https://pluggy.readthedocs.io/en/latest/))
* resilient runtime (failfast and startfast)
* automated documentation ([sphinx](http://www.sphinx-doc.org/en/master/))

The complete demo consists of the following repositories:

* Photon Development ([nytimes/photon-dev_demo](https://github.com/nytimes/photon-dev_demo))
* Photon Application ([nytimes/photon-demo-util](https://github.com/nytimes/photon-demo-util))
* Photon Common ([nytimes/photon-common-demo](https://github.com/nytimes/photon-common-demo))

## Getting Started

Follow these steps to get started with exploring and running the Photon development environment, application, and common library.

### Prerequisites

1. The code runs within macOS and Ubuntu environments, but has not been tested in Windows environments.

2. At least [Python v3.7](https://www.python.org/downloads/)

3. The application uses Wand, an [ImageMagick](http://www.imagemagick.org/) binding for Python. To use the library, you will need ImageMagick installed:

```
brew install imagemagick
```

### Installation

1. Clone this repository (this will be the parent directory):

```
git clone https://github.com/nytimes/photon-dev_demo.git
```

2. Copy `bin/photon` into `/usr/local/bin`:

```
cd photon-dev_demo
cp bin/photon /usr/local/bin
```

3. Clone Photon family projects into this project (as subdirectories):

```
git clone https://github.com/nytimes/photon-demo-util.git
git clone https://github.com/nytimes/photon-common-demo.git
```

4. Within any part of the "**MONTY**" structure, activate the development environment:

```
source photon
```

5. Now you're ready to explore and learn more [about the system](#about-the-system).

### Checking code quality

Within each subproject, use the following commands to check code quality and formatting. The **flake8** `--max-line-length=88` option makes it compatible with **black**'s default formatting. For **mypy**, pass in each path you'd like to check that contains Python files.

* `cd <subproject>`
* `black --check .`
* `flake8 --max-line-length=88`
* `mypy --strict --namespace-packages --ignore-missing-imports <subproject dirs>`

### Exiting / cleaning up

If you'd like to retain the virtual environment that `source photon` built, simply `exit`. Running `source photon` again will reinstantiate the venv.

If you'd like to delete the venv and reinstall it, `exit`, run `rm -rf venv` from within `photon-dev_demo`, and `source photon` again.

## About the System

### Photon Development

Photon Development (`photon-dev_demo`) provides the umbrella project for the local development and documentation of the Photon family of applications, and includes:

* A shell script (`bin/photon`) for local development of packages; and
* **Sphinx** documentation builder for Photon family projects.

The `photon` shell script, when 'sourced' (`source photon`), will create a shared virtual environment in 'develop' mode, i.e. `pip install --editable ... -r requirements.txt -r requirements_dev.txt`, for all the `photon-*` subprojects based in the `photon-dev_demo` directory. It will also install the requirements for building documentation.

The `docs/` directory has the objects necessary to do a **sphinx** build of the overview docs and the docs for subprojects. Note that this is done via linking in `index.rst` and folder links to the `docs/` directories in the subprojects. All subprojects should follow this pattern.

#### Generating Docs

1. `cd docs/` then `make html` will build the html version
2. `open _build/html/index.html` will display it in your default browser

You can also do this at the subproject level, but the resulting html will only include the docs for that subproject. Generating docs at the `photon-dev_demo` level will include all subprojects that are properly linked.

### Photon Application

Photon Application (`photon-demo-util`) is a simple implementation of a Photon application structure - thread-based and invokable via a command-line utility. Each application's `setup.py` defines a [console script](https://github.com/nytimes/photon-demo-util/blob/master/setup.py#L28) entrypoint that invokes the **click** CLI for running and interacting with the application:

```
demo_util --help
demo_util demo --help
demo_util demo -e
```

Following the code from the entrypoint onwards will allow you to get familiar with the application structure and how it utilizes threads. The application implements **failfast** and **startfast**, and uses **pluggy**-based plugins for image transforms. Comments throughout the code provide additional context.

The application will create a `demo` directory on your Desktop - putting images in `incoming` will allow them to be processed while the application runs. Images transformed by the plugins will go to `modified`, original images will go to `original`, and images rejected (e.g., not a JPEG) will not be modified and will be moved to `rejected`.

Each Photon application would contain its own `Dockerfile` and be deployed individually.

### Photon Common

Photon Common (`photon-common-demo`) provides examples of shared classes and methods for Photon applications, such as utilities for logging and interacting with time-based objects. This is also where we include wrappers around often-used libraries and tools such as `exiftool`.

Photon common libraries can be packaged and distributed individually - Photon applications can then add them to their `requirements.txt`.

## Questions, Comments, or Feedback

If you have any questions, comments, or feedback, feel free to reach out by adding an issue [here](https://github.com/nytimes/photon-dev_demo/issues)!
