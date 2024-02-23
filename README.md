# Test `pip` install Action

This action runs install wheels using `pip` to test the installation of
a package.

Here is an example demonstrating how to use it in a workflow:

```yaml
jobs:
  test-installation:
    name: Test installation
    runs-on: ubuntu-20.04

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Build wheels
        uses: frequenz-floss/gh-action-build-python@v0.x.x
      - name: Test installation
        uses: frequenz-floss/gh-action-test-pip-install@v0.x.x
        with:
          python_version: "3.11"
```

> [!TIP]
> If you need to test the installation in a different architecture, you can use
> the
> [`gh-action-test-pip-install-cross-arch`](https://github.com/frequenz-floss/gh-action-test-pip-install-cross-arch)
> action.

## Inputs

* `python_version`: The Python version to use. Required.

   This is passed to the `actions/setup-python` action.

* `wheel_files`: The wheel files to install. Default: `dist/*.whl`.

   It is expected that the wheel files already exist.  You can use the
   [`frequenz-floss/gh-action-build-python`](https://github.com/frequenz-floss/gh-action-build-python)
   action to build the wheels.

## Recommended use with matrix jobs

When using a matrix, it is recommended to create a dummy job to *merge* all the
matrix jobs, specially if you want to require all matrix jobs to pass to allow
merging a pull request. If you do this, you only need to add the dummy job as
a requirement and you don't need to update your requirements each time you
update your matrix.

```yaml
  test-intallation-all:
    # The job name should match the `name` of the `test-installation` job.
    name: Test installation
    needs: ["test-installation"]
    runs-on: ubuntu-20.04
    steps:
      - name: Return true
        run: "true"
```
