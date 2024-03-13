# non-package organization


## overview

- [`nwb.py`](nwb.py) imports [`utils.py`](utils.py) and uses one of its functions
- all required dependenices need to be installed manually
    - ideally a `requirements.txt` should be provided


## setup
Change directory into this folder and create a venv:
```
python -m venv .venv
```
Activate venv:
```
.venv\scripts\activate
```
Install required packages:
```
pip install h5py zarr remfile npc_io
```

## usage

While our current working directory contains the .py files, we can import and use them:

```python
import nwb
import utils

path = 's3://codeocean-s3datasetsbucket-1u41qdg42ur9/39490bff-87c9-4ef2-b408-36334e748ac6/nwb/ecephys_620264_2022-08-02_15-39-59_experiment1_recording1.nwb'

session = nwb.LazyNWB()

remote_zarr_file = utils.open(path)
```

However, if we're working in any other directory, this import in [`nwb.py`](nwb.py), will fail:
```python
import utils
```
as Python looks for a file called `utils` in the current directory or on
its global path:

```python
>cd .. && python -c "from project_0 import nwb"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "C:\Users\ben.hardcastle\github\cat_packaging\project_0\nwb.py", line 11, in <module>
    import utils
ModuleNotFoundError: No module named 'utils'
```

Bundling these files into a package will make it possible to use them anywhere,
regardless of our current working directory.