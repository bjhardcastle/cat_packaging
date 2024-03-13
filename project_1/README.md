# package organization

## overview
- .py files moved to `src/lazynwb`
    - `lazynwb` will be our top-level package: the thing we import in Python
    - the `src`-level folder is not required, but is recommended
        - Python adds everything in the current working directory to its path -
          when we refer to `lazynwb`, we want to make sure that Python and other
          tools know we mean the package rather than the folder
- an empty [`__init__.py`](src/lazynwb/__init__.py) has been added 
    - this signals to Python and other tools that `lazynwb` is a package
    - it can be used to define what is available when `lazynwb` is imported
- [`pyproject.toml`](pyproject.toml) has been added to the root of the project and
  filled out
    - this location is important: it should not live in `src`
- [`nwb.py`](src/lazynwb/nwb.py) was modified:
    ```python
    import utils
    ```
    became:
    ```python
    import lazynwb.utils as utils
    ```
    - it no longer relies on a relative import, instead using the absolute "path" to
      `utils` within the `lazynwb` folder
    - note: `import lazynwb.utils` works fine too - it would just require doing a
      find+replace on `utils` -> `lazynwb.utils`

## setup
Change directory into this folder and create a venv:
```
python -m venv .venv
```
Activate venv:
```
.venv\scripts\activate
```
Editable install the packaged project:
```
pip install -e .
```

- editable install makes the package available for import
- the package is built and installed according to the metadata in [`pyproject.toml`](pyproject.toml)
- editing the source code will take effect the next time the package is imported:
  no need to re-run the install command

## usage

With our venv activated, we can access individual modules by their absolute path
within the package:

```python
import lazynwb.nwb
import lazynwb.utils

path = 's3://codeocean-s3datasetsbucket-1u41qdg42ur9/39490bff-87c9-4ef2-b408-36334e748ac6/nwb/ecephys_620264_2022-08-02_15-39-59_experiment1_recording1.nwb'

session = lazynwb.nwb.LazyNWB(path)

remote_file = lazynwb.utils.open(path)
```

## curating the user experience

For convenience, we can instead provide everything contained in the package at the
"root" level, so users don't need to remember where things live:

Add the following to [`__init__.py`](src/lazynwb/__init__.py):
```python
from lazynwb.nwb import *
from lazynwb.utils import *
```

then we can access everything with one import:
```python
import lazynwb

path = 's3://codeocean-s3datasetsbucket-1u41qdg42ur9/39490bff-87c9-4ef2-b408-36334e748ac6/nwb/ecephys_620264_2022-08-02_15-39-59_experiment1_recording1.nwb'

session = lazynwb.LazyNWB(path)

remote_file = lazynwb.open(path)
```

Caution: if there are multiple objects with the same name in the package, it will be
ambiguous which one is imported! If you're going to do this, everything needs a unique name.

The more-correct thing to do here is to import each object in
`__init__.py` individually, and add them **as strings** to the reserved-variable `__all__`:
```python
from utils import open
from nwb import LazyNWB

__all__ = [
    "open",
    "LazyNWB",
]
```
I just find this too much effort during early development, when things are
added/renamed often. A case of *"practicality beats purity"*.

## dependency management
One of the dependencies in the project specifies its own dependencies as follows: 
[`npc_io/pyproject.toml`](https://github.com/AllenInstitute/npc_io/blob/47c2a685d9f32733736b48b0c1d639b5e6cf77b9/pyproject.toml#L11):
```
dependencies = [
    ...
    "universal-pathlib<0.2.0",
    ...
]
```
but pip will happily install `"universal-pathlib==0.2.0"` for us, and likely break something in `npc_io`, because it
has no efficient way of checking the dependency specifications of every package
our project uses.

There are popular third-party tools that *do* check compatibility between dependencies, like
[`poetry`](https://python-poetry.org/) and
[`pdm`](https://pdm-project.org/latest/).

It's highly recommended that you use one:
- incorrect or vague dependency specification makes code re-use more difficult
- keeping track of every sub-dependency is too much effort

`pdm` is recommended over `poetry`, as it follows 
the standards for metadata in `pyproject.toml`, so your projects aren't
locked into using any particular tool in the future.