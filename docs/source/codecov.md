# Codecov Tutorial

[Codecov](https://about.codecov.io/) is a powerful tool to calculate the code coverage from test, and it is recommend to use it with [Github Actions](https://docs.github.com/cn/actions).

To use the `Codecov` and `Github Actions`, you should prepare some test files first, and we recommend you perform the tests using the package [pytest](https://docs.pytest.org/en/7.1.x/). Detailed information about the test files writing obvious exceed the content of this tutorial.

Actually, the automatic test by `Github Actions` only need to add a `*.yaml` file under the `.github/workflows`. A simple `codecov.yaml` to test [GVasp](https://github.com/Rasic2/gvasp) is like this:

```yaml
name: Codecov
on: push
jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Set up Python 3.9
        uses: actions/setup-python@v2
        with:
          python-version: "3.9"
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Generate lib files
        run: |
          python setup.py build
          find ./build -iname '*.so' -exec cp {} ./gvasp/lib \;
      - name: Modify config
        run: |
          python tests/modify_config.py
      - name: Copy files
        run: |
          cp tests/*.xsd .
          cp tests/XDATCAR .
          cp tests/OUTCAR .
          cp tests/CONTCAR .
          cp tests/EIGENVAL .
          cp tests/AECCAR* .
          cp tests/CHGCAR .
          cp tests/CONTCAR_dos tests/DOSCAR_dos .
      - name: Run tests and collect coverage
        run: |
          pip install pytest
          pip install pytest-cov
          pytest --cov=./ --cov-report=xml
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
```

It can be simplify as the following steps:

- Set up job
- Checkout
- Set up Python 3.9
- Install dependencies
- Generate lib files
- Modify config
- Copy files
- Run tests and collect coverage
- Upload coverage to Codecov
- Post Set up Python 3.9
- Post Checkout
- Complete job

In fact, how to perform the automatic test depends on your project, for example, the `generate lib files`, `modify config` and `copy files` are mostly will occur for your project.

The detailed information about the arguments in `*.yaml` can see [here](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions).
