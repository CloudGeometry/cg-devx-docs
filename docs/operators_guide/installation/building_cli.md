# Building CG DevX CLI

This will allow you to build CG DevX CLI from sources

## Pre-requisites

You should have:

- **python 3.10 + pip**
- **[poetry](https://python-poetry.org/)** 1.6.*

If you don't have poetry installed, please follow official installation
instructions [here](https://python-poetry.org/docs/#installation).

## Preparing environment

```bash
# Assumed directory: GITROOT/tools
# NOTE: Poetry configuration and lock files are stored in the 'cli' directory.

# To install dependencies, use:
poetry install

# Activate the virtual environment with:
# By default, Poetry creates a virtual environment in {cache-dir}/virtualenvs
poetry shell
```

To find more on poetry commands, please [see](https://python-poetry.org/docs/basic-usage/).

## Building CLI tool

To build CLI tool, please run `PyInstaller`

directly

```bash 
# Current directory: GITROOT/tools
python -m PyInstaller --onefile cli/__main__.py --name cgdevxcli
```

or via python script

```bash 
# Current directory: GITROOT/tools
poetry run build
```

After that you could use and distribute `cgdexvcli` located at `GITROOT/dist/cgdevxcli`
