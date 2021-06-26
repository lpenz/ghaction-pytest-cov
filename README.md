[![CI](https://github.com/lpenz/ghaction-pytest/actions/workflows/ci.yml/badge.svg)](https://github.com/lpenz/ghaction-pytest/actions/workflows/ci.yml)

# ghaction-pytest

Github action that runs pytest --cov after installing the
package's test dependencies.

This is a very simple github action that should be used along with
[setup-python](https://github.com/marketplace/actions/setup-python) and
[AndreMiras/coveralls-python-action](https://github.com/marketplace/actions/coveralls-python).


## Inputs

### `working-directory`

Go to this directory before running pytest.

Defaults to the current directory.


## Examples

Example repository that uses this action: https://github.com/lpenz/ftpsmartsync/


### Simple workflow

```.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
      - uses: lpenz/ghaction-pytest@v1
```


### Workflow with python version matrix and coveralls

```.yml
jobs:
  test:
    strategy:
      matrix:
        python-version:
          - 3.6
          - 3.7
          - 3.8
          - 3.9
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: ${{ matrix.python-version }}
      - uses: lpenz/ghaction-pytest@v1
      - uses: AndreMiras/coveralls-python-action@v20201129
        with:
          parallel: true
          flag-name: python-${{ matrix.python-version }}
          path-to-lcov: .coverage
          github-token: ${{ secrets.GITHUB_TOKEN }}
  coverage-finish:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: AndreMiras/coveralls-python-action@v20201129
        with:
          parallel-finished: true
```
