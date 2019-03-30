# Build AWS Lambda Layer zip file for Python Dependancies

Creates an AWS Lambda Layers **optimized** zip file using the [Lambda Layer directory structure](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html#configuration-layers-path), ensures compiled libraries are compatible with Lambda environment, and optimized to reduce file size.

This function was created to address these issues:

- Many methods of creating Lambda zip files for Python functions don't work for Lambda Layers because Lambda Layers require specific library paths within the zip
- Some dependancies required compiled components, which requires that the zip is created in an environment that matches the Lambda runtime environment
- Minimize the zip size by removing unnecessary items

**Note: This script requires Docker and uses a container to mimic the Lambda environment.**

## Features

- Builds zip file containing Python dependancies and places the libraries into the proper directory structure for lambda layers
- Ensures compiled libraries are compatible with Lambda environment by using [docker container](https://hub.docker.com/r/lambci/lambda) that mimics the lambda runtime environment
- Optimized the zip size by removing `.pyc` files and unnecessary libraries
- allows specifying lambda supported python versions: 2.7, 3.6 and 3.7
- Automatically searches for requirements.txt file in several locations:
  - same directory as script
  - parent directory or script (useful when used as submodule)
  - function sub-directory of the parent directory

## Installation

This function can be **cloned** for standalone use, into a parent repo or added as a **submodule**.

Clone for standalone use or within a repo:

``` bash
# If installing into an exisiting repo, navigate to repo dir
git clone --depth 1 https://github.com/robertpeteuil/build-lambda-layer-python _build_layer
```

Alternatively, add as a submodule:

``` bash
# PUBLIC USE HTTPS
cd {repo root}
# eventual public repo will use http
git submodule add https://github.com/robertpeteuil/build-lambda-layer-python _build_layer
# Update submodule
git submodule update --init --recursive --remote
```

## Use

- Run the builder with the command `./build_layer.sh`
- Optionally specify Python Version
  - `-p PYTHON_VER` - specifies the Python version: 2.7, 3.6, 3.7 (default 3.6)
- It uses the first requirements.txt file found in these locations (in order):
  - same directory as script
  - parent directory of script (useful when used as submodule)
  - function sub-directory of the parent directory (useful when used as submodule)

## Reference - remove submodule

If installed as submodule and want to remove

``` bash
# Remove the submodule entry from .git/config
git submodule deinit -f $submodulepath
# Remove the submodule directory from the superproject's .git/modules directory
rm -rf .git/modules/$submodulepath
# Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
git rm -f $submodulepath
# remove entry in submodules file
git config -f .git/config --remove-section submodule.$submodulepath
```
