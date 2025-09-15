galaxyproject.miniconda
=======================

An [Ansible][ansible] role for installing and managing [Conda][conda] using [Miniforge][miniforge], [micromamba][micromamba], [Miniconda][miniconda] or [Anaconda][anaconda]. Additionally, the role can
manage the creation of a [Conda][conda] environment that can be used to create a [venv][venv] for [Galaxy][galaxy].

> [!WARNING]
> Use of Miniconda and Anaconda by those at organizations with more than 200 employees or contractors (including Affiliates) requires a [paid Anaconda Business license](https://www.anaconda.com/pricing).

[ansible]: https://www.ansible.com/
[miniforge]: https://github.com/conda-forge/miniforge
[micromamba]: https://github.com/mamba-org/micromamba-releases
[miniconda]: https://www.anaconda.com/docs/getting-started/miniconda/
[anaconda]: https://www.anaconda.com/docs/getting-started/anaconda/
[conda]: https://docs.conda.io/en/latest/
[venv]: https://docs.python.org/3/tutorial/venv.html
[galaxy]: https://galaxyproject.org/

Requirements
------------

A [Conda][conda]-compatible version of Linux or macOS is required.

Role Variables
--------------

See [defaults/main.yml](defaults/main.yml) for a full list.

The only required variable is `miniconda_prefix`, the root of the Conda installation.

The following two variables can be used to select the Conda installer and executable:

- `miniconda_distribution`: the values can be `miniforge` (default), `micromamba`, `miniconda` or `anaconda`
- `miniconda_executable`: the default value depend on the value of `miniconda_distribution`
  - `mamba`: when the distribution is _miniforge_
  - `micromamba`: when the distribution is _micromamba_
  - `conda`: when the distribution is _miniconda_ or _anaconda_

To create arbitrary conda environments, use the variable `miniconda_conda_environments` as shown in the defaults, or the
example below. The role will also update these environments if you change their list of packages or package versions.

To create an env named `_galaxy_` for creating a venv for [Galaxy][galaxy], set `galaxy_conda_create_env` to `true`. You
can then use `{{ miniconda_prefix }}/envs/_galaxy_/bin/virtualenv` as the value to `galaxy_virtualenv_command` in
[galaxyproject.galaxy][galaxy-role]. This is particularly useful if you need a cross-platform copy of Python and
Galaxy's venv to be deployed on a shared filesystem between a Galaxy server and cluster that may not have matching OS
distributions and versions.

[galaxy-role]: https://github.com/galaxyproject/ansible-galaxy

Terms of Service (ToS) for Conda Channels
+++++++++++++++++++++++++++++++++++++++++

Some Conda channels require explicit acceptance of their Terms of Service (ToS).

Use the `miniconda_tos_channels` variable to list the channels for which the role should pre-accept the ToS:

```yaml
miniconda_tos_channels:
  - https://repo.anaconda.com/pkgs/main
  - https://repo.anaconda.com/pkgs/r
```

These are currently required when `miniconda_distribution: miniconda`.

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: localhost
  vars:
    miniconda_prefix: /conda           # required
    miniconda_distribution: miniforge  # optional
    miniconda_executable: mamba        # optional
    miniconda_conda_environments:
      python@3.9:
        channels:                     # optional, defaults to miniconda_channels
          - conda-forge
          - defaults
        packages:
          - python=3.9
  connection: local
  roles:
     - galaxyproject.miniconda
```

License
-------

MIT

Author Information
------------------

[View contributors on GitHub](https://github.com/galaxyproject/ansible-miniconda/graphs/contributors)
