### Overview

- **`venv`** can only create isolated Python environments, while **`conda`** can create and manage isolated environments for any supported programming language.
- **`pip`** installs only Python packages from [PyPI](https://pypi.org/). In contrast, **`conda`** can install packages (written in any language) from various repositories such as the Anaconda Repository and Anaconda Cloud. You can also install PyPI packages by running `pip` within a Conda environment.
- **`conda`** is both a package manager and an environment manager, and it does not depend on any single programming language.

---

### Conda Commands for Searching and Installing 

Use the following commands to search for packages in Conda repositories:

```zsh
conda search <package_name>
conda search --override-channels -c <channel_name> <package_name>
conda search -f <package_name>  # Use --full-name as a long form of -f
```

To install packages:

```zsh
conda install <package_name>[=<version>]
conda install --channel conda-forge <package_name>[=<version>]
```

> **Note**: If you do not specify an environment name (e.g., `-n <env_name>`), Conda installs the package into the current active environment.

---

### Channels

- **Channels** are the locations (repositories) where Conda looks for packages.
- Conda checks channels in a hierarchical order: the highest-priority channel is searched first. You can change the priority or add new channels.
- It is often recommended to add a new channel with the lowest priority, so you can still use "special" packages from that channel when needed, without affecting the default channels from Continuum (the Anaconda defaults).

---

### Updating Conda

```zsh
conda update conda
conda update --all
conda update -n <env_name> conda
```

---

### Creating Environments

```zsh
conda create -n <env_name> <package_name>
conda create --no-default-packages --no-deps -n <env_name> <package_name>[=<version>]
```

### Activating and Deactivating Environments

```zsh
conda activate <env_name>
conda deactivate
```

### Listing Environments

```zsh
conda env list
conda info -e   # or conda info --envs
```

---

### Checking Environment Details in Python

In a Python session, you can check details of the current environment versus the base Python installation:

```python
import sys
sys.prefix, sys.exec_prefix  # Point to the prefix of the current (virtual) environment.
sys.base_prefix, sys.base_exec_prefix  # Point to the base Python installation.
```

For more information about your Conda setup:

```zsh
conda info
```

---

### Listing and Inspecting Packages

```zsh
conda list
```

Inside Python:

```python
import sys
sys.modules.keys()  # Shows all currently loaded modules.
```

To export all installed packages:

```zsh
conda list -e > requirements.txt
```

And to install from that file:

```zsh
conda install -n <env_name> --file requirements.txt
```

---

### Conda Configuration

```zsh
conda config --show
conda config --set <key> <value>
conda config --append <key> <value>
conda config --get  # get all keys and values from .condarc
conda config --get channels  # show the current channel priority
conda config --add channels <channel_name>  # add a new channel
```

---

### Removing Packages and Cleaning Up

```zsh
conda uninstall <package_name>
conda remove -n <env_name> --all  # remove the environment entirely
```

To clean up (remove cached .tar.bz2 files):

```zsh
conda clean -t
```

---

### Exporting and Creating Environments via YAML

```zsh
conda env export > environment.yml
conda env create -f environment.yml
```

---

### Working with Jupyter Kernels

Check available kernels:

```zsh
jupyter kernelspec list
```

Install the `ipykernel` package, create a Conda environment, activate it, and install its kernel:

```zsh
conda install ipykernel
conda create -n <env_name> python=<version>
conda activate <env_name>
python -m ipykernel install --user -n <kernel_name>
conda deactivate
```

Then start Jupyter Notebook:

```zsh
jupyter notebook
```

---

### Checking Versions

Check Conda version:

```zsh
conda -V
conda --version
```

Check Python version:

```zsh
python -V
```

Or from within Python:

```python
import sys
sys.version
```
