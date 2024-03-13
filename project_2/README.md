# pdm organization

## setup
```
pip install pdm
pdm init
```

- answer the setup questions
- add code to `src/<project_name>`
- add dependencies with `pdm add h5py zarr remfile npc_io`

## notes
- minimum versions are added to [`pyproject.toml`](pyproject.toml)
- exact versions used in development environment are specified in
  [`pdm.lock`](pdm.lock)
    - the lock file should always be committed so others/you can re-create the dev
      environment exactly
- packages must no longer be pip installed 
    - it would subvert the compatibility checks that `pdm` does
    - they wouldn't be added to `pyproject.toml` or `pdm.lock`
- `pdm` won't fetch new versions of Python for you, like `conda`:
    - on Windows, the only versions available on `pdm init` are those already
      somewhere on your system
    - on Linux, `pdm` can use `pyenv` to fetch arbitrary versions of Python 
    
## installing an existing project
To recreate an environment for an existing project that used `pdm`:
```
cd ..
git clone https://github.com/bjhardcastle/lazynwb
cd lazynwb
pdm install
```

## publishing 
After manually updating the version in [`pyproject.toml`](pyproject.toml):
```
pdm publish
```