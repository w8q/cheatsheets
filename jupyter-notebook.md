<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Autoreload](#autoreload)
- [Display all results](#display-all-results)
- [`matplotlib`](#matplotlib)
  - [`import` method](#import-method)
  - [Edit config file at `~/.ipython/profile_default/ipython_config.py`](#edit-config-file-at-ipythonprofile_defaultipython_configpy)
- [Debugger (`pdb.set_trace` replacement)](#debugger-pdbset_trace-replacement)
- [`nbextensions`](#nbextensions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### Autoreload

```python
%reload_ext autoreload
%autoreload 2
```

---

### Display all results

```python
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```

---

### `matplotlib`

#### `import` method

```python
import matplotlib.pyplot as plt
%matplotlib inline
```

#### Edit config file at `~/.ipython/profile_default/ipython_config.py`

```python
c = get_config()
c.InteractiveShellApp.matplotlib = "inline"
```

---

### Debugger (`pdb.set_trace` replacement)
```
from IPython.core.debugger import set_trace
```

---

### `nbextensions`

https://stackoverflow.com/questions/33159518/collapse-cell-in-jupyter

Install `nbextensions`

```sh
$ pip install jupyter_contrib_nbextensions
$ jupyter contrib nbextension install --user
```

Install `configurator`

```sh
$ pip install jupyter_nbextensions_configurator
$ jupyter nbextensions_configurator enable --user
```
