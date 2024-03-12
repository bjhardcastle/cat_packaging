# non-package organization


## overview
- `nwb.py` uses a function from `utils.py`
- all required dependenices need to be manually installed 


## setup
```
python -m venv .venv
.venv\scripts\activate
pip install h5py zarr remfile npc_io
```

## using functions

While current working directory contains the .py files:

```python
import nwb
import utils

path = 's3://codeocean-s3datasetsbucket-1u41qdg42ur9/39490bff-87c9-4ef2-b408-36334e748ac6/nwb/ecephys_620264_2022-08-02_15-39-59_experiment1_recording1.nwb'

session = nwb.LazyNWB()

remote_zarr_file = utils.open(path)
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