# Export poetry to legacy formats

Do you like using [poetry](https://python-poetry.org) for packaging, but also like being able to install a project with `pip install -e`? This action uses `poetry` and `create_setup.py` (from [flicamera](https://github.com/sdss/flicamera)) to generate `setup.py` and `requirements.txt` from `pyproject.toml`.

This can be combined with [create-pull-request](https://github.com/marketplace/actions/create-pull-request) to keep `setup.py` and `requirements.txt` in sync with `pyproject.toml`.

```yaml
name: ci

on:
  push:
    branches-ignore:
      update-setup

jobs:
  artifacts:
    runs-on: ubuntu-18.04

    steps:
    - uses: actions/checkout@v2
    - uses: actions/setup-python@v2
      with:
        python-version: 3.8
    - uses: jvansanten/poetry-export-to-legacy-action@master
    - name: Create Pull Request
      uses: peter-evans/create-pull-request@v3
      with:
        commit-message: Update legacy package
        committer: GitHub <noreply@github.com>
        author: ${{ github.actor }} <${{ github.actor }}@users.noreply.github.com>
        signoff: false
        branch: update-setup
        delete-branch: true
        title: 'Update setup.py and friends'

```
