# package organization

## overview
- .py files moved to `src/lazynwb`
- empty `__init__.py` has been added 
- `pyproject.toml` has been added and filled out
- imported `utils` from package in [`nwb.py`](src/lazynwb/nwb.py)

## setup
Create venv:
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
- editing the source code will take effect next time Python runs

## using functions

With venv activated, we access functions/classes with absolute path within package:

```python
import lazynwb.nwb
import lazynwb.utils

path = 's3://codeocean-s3datasetsbucket-1u41qdg42ur9/39490bff-87c9-4ef2-b408-36334e748ac6/nwb/ecephys_620264_2022-08-02_15-39-59_experiment1_recording1.nwb'

session = lazynwb.nwb.LazyNWB(path)

remote_file = lazynwb.utils.open(path)
```

## curating the user experience

For convenience, we can provide everything at the "root" level, so users don't need to
remember where things live:

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


## dependency management
[`npc_io/pyproject.toml`](https://github.com/AllenInstitute/npc_io/blob/47c2a685d9f32733736b48b0c1d639b5e6cf77b9/pyproject.toml#L11)
specifies:
```
dependencies = [
    ...
    "universal-pathlib<0.2.0",
    ...
]
```
but pip will happily install `"universal-pathlib==0.2.0"` for us, which will
likely break something in `npc_io`, because it
has no efficient way of checking the dependency specifications of every package
our project uses.

However, there are popular third-party tools that *do* check compatibility between dependencies, like
[`poetry`](https://python-poetry.org/) and
[`pdm`](https://pdm-project.org/latest/). 

`pdm` is recommended over `poetry`, as it follows 
the standards for metadata in `pyproject.toml`, so your projects aren't
locked into using any particular tool in the future.