# non-package organization


## overview
- `nwb.py` uses a function from `utils.py`
- all required dependenices need to be manually installed 


## setup
Create venv:
```
python -m venv .venv
```
Activate venv:
```
.venv\scripts\activate
```
Installa required packages:
```
pip install h5py zarr remfile npc_io
```

## using functions

While our current working directory contains the .py files, we can import them:

```python
import nwb
import utils

path = 's3://codeocean-s3datasetsbucket-1u41qdg42ur9/39490bff-87c9-4ef2-b408-36334e748ac6/nwb/ecephys_620264_2022-08-02_15-39-59_experiment1_recording1.nwb'

session = nwb.LazyNWB()

remote_zarr_file = utils.open(path)
```
