# PIP Installer v2

[![SuperLinter](https://github.com/insightsengineering/pip-action/actions/workflows/linter.yml/badge.svg)](https://github.com/insightsengineering/pip-action/actions/workflows/linter.yml)
[![Test 🧪](https://github.com/insightsengineering/pip-action/actions/workflows/test.yml/badge.svg)](https://github.com/insightsengineering/pip-action/actions/workflows/test.yml)

This action installs pip packages.

**NOTE:** This is a fork of <https://github.com/BSFishy/pip-action>. The original action has been unmaintained for a while. This fork is actively maintained by the Roche/Genentech Insights Engineering Team.

## Usage

See [action.yml](action.yml)

Basic:

```yaml
steps:
- uses: actions/checkout@v3
- uses: actions/setup-python@v4
- uses: insightsengineering/pip-action@v2
  with:
    packages: |
      package1
      package2
- run: python main.py
```

All options:

```yaml
- uses: insightsengineering/pip-action@v2
  with:
    packages: package
    requirements: requirements.txt
    constraints: constraints.txt
    no-deps: true
    pre: true
    editable: editable
    platform: ':all:'
    upgrade: true
    extra: extra options
```

### Options

Here is a list of all of the available options supported by this action.

#### `packages`

The packages to install.
This option can replace the `requirements` input or `editable` input, but either this, the `requirements` option, or the `editable` option is required.

A list can be provided to specify multiple packages.
This syntax is a little different because actions only support string inputs.
Due to this, each individual package can have no whitespace except around it.

This option does not correspond to any additional command flags, as none are necessary.

**Examples:**

```yaml

## Valid inputs
packages: package1         # python -m pip install package1
packages: package1==1.0.0  # python -m pip install package1==1.0.0

packages: package1 package2                # python -m pip install package1 package2
packages: package1==1.0.0 package2         # python -m pip install package1==1.0.0 package2
packages: package1==1.0.0 package2==1.2.3  # python -m pip install package1==1.0.0 package2==1.2.3

# python -m pip install package1
packages: |
  package1
# python -m pip install package1==1.0.0
packages: |
  package1==1.0.0
# python -m pip install package1 package2
packages: |
  package1
  package2
# python -m pip install package1==1.0.0 package2
packages: |
  package1==1.0.0
  package2
# python -m pip install package1==1.0.0 package2==1.2.3
packages: |
  package1==1.0.0
  package2==1.2.3

## Invalid inputs
packages: package1 == 1.0.0

packages: |
  package1 == 1.0.0
```

#### `requirements`

A [requirements file](https://pip.pypa.io/en/stable/user_guide/#requirements-files) to install from.
This option can replace the `packages` input or `editable` input, but either this, the `packages` option, or the `editable` option is required.

The input value should be a valid requirements file to install from.
It corresponds to the `--requirement` argument for pip.
Any valid value for that argument will be valid for this option.

No special formatting is done with this input, so it won't be treated special.
The value recieved from this input will be passed directly to pip.

**Examples:**

```yaml
requirements: ''                    # python -m pip install package1
requirements: requirements.txt      # python -m pip install --requirement requirements.txt
requirements: src/requirements.txt  # python -m pip install --requirement src/requirements.txt
```

#### `constriants`

A [constraints file](https://pip.pypa.io/en/stable/user_guide/#constraints-files) to install from.
This can be used to specify the versions of packages that are allowed to be installed.
It does **not** do anything to actually install new packages.

The input should be a valid constraints file to use.
It corresponds to the `--constraint` argument for pip.
Any valid value for that argument will be valid for this option.

No special formatting is done with this input, so it won't be treated special.
The value recieved from this input will be passed directly to pip.

**Examples:**

```yaml
constraints: ''                   # python -m pip install package1
constraints: constraints.txt      # python -m pip install --constraint constraints.txt package1
constraints: src/constraints.txt  # python -m pip install --constraint src/constraints.txt package1
```

#### `no-deps`

Specify not to install package dependencies.
This will cause only the packages specified to be installed, and none of their dependencies.

It corresponds to the `--no-deps` argument for pip.

It is a boolean input, so either `true` or `false` will work as inputs.

**Examples:**

```yaml
no-deps: true   # python -m pip install --no-deps package1
no-deps: false  # python -m pip install package1
```

#### `pre`

Specify to install development or pre-release versions of packages.
This will allow the latest version of the package to be isntalled rather than the stable version.

It corresponds to the `--pre` argument for pip.

It is a boolean input, so either `true` or `false` will work as inputs.

**Examples:**

```yaml
pre: true   # python -m pip install --pre package1
pre: false  # python -m pip install package1
```

#### `editable`

Install a package in editable mode.
This option can replace the `packages` input or `requirements` input, but either this, the `packages` option, or the `requirements` option is required.

The input should be a valid package to install.
It corresponds to the `--editable` argument for pip.
Any valid value for that argument will be valid for this option.

No special formatting is done with this input, so it won't be treated special.
The value recieved from this input will be passed directly to pip.

**Examples:**

```yaml
editable: ''                                              # python -m pip install package1
editable: path/to/SomeProject                             # python -m pip install --editable path/to/SomeProject
editable: git+http://repo/my_project.git#egg=SomeProject  # python -m pip install --editable git+http://repo/my_project.git#egg=SomeProject
```

#### `platform`

Only install packages compatible with a specific platform.
This will default to the platform currently running.

The input should be a valid platform.
It corresponds to the [`--platform` argument for pip](https://pip.pypa.io/en/stable/reference/pip_install/#cmdoption-platform).
Any valid value for that argument will be valid for this option.

No special formatting is done with this input, so it won't be treated special.
The value recieved from this input will be passed directly to pip.

**Examples:**

```yaml
platform: ''                         # python -m pip install package1
platform: linux_x86_64               # python -m pip install --platform linux_x86_64 package1
platform: linux_ubuntu_18_04_x86_64  # python -m pip install --platform linux_ubuntu_18_04_x86_64 package1
```

#### `upgrade`

Specify to upgrade packages.
This will cause already installed packages to be upgraded if an upgrade is available.

It corresponds to the `--upgrade` argument for pip.

It is a boolean input, so either `true` or `false` will work as inputs.

**Examples:**

```yaml
upgrade: true   # python -m pip install --upgrade package1
upgrade: false  # python -m pip install package1
```

#### `extra`

Specify additional arguments to pass to pip.
These are extra options that may not be provided by independent inputs to this action.

It does not correspond to any pip arguments.
The input is passed directly to pip without any formatting.

**Examples:**

```yaml
extra: ''      # python -m pip install package1
extra: --user  # python -m pip install --user package1
```

### Additional info

Here is just a little bit of additional information to explain some things that may not be trivial.

#### Python location

It is highly recommended that you use the [setup-python](https://github.com/actions/setup-python) action to install Python.
This streamlines the process and makes sure that you have a valid Python installed and located.

This action will automatically detect if Python is installed in 1 of two ways.
Firstly, it checks the `pythonLocation` environment variable.
This is set by the `setup-python` action, and is a directory pointing to the Python installation.

Secondly, if the `setup-python` action is not used, the path will be checked for Python.
If Python is found in the path environment variable, this action will use that version.
Otherwise, an error is thrown.

#### Empty strings

Empty strings are used to signify no input.
Github Actions only allow string key/value inputs, so inputs are always strings.
Inputs that are not specified automatically return an empty string.
Because of this, we use an empty string to signify an input has no value.
For most options, this will cause the argument to not appear in the final command.

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
