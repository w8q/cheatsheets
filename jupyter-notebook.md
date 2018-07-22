<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Autoreload](#autoreload)
- [Display all results](#display-all-results)
- [`nbextensions`](#nbextensions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---

### Autoreload
```
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
```
import matplotlib.pyplot as plt
%matplotlib inline
```

### Edit config file at `~/.ipython/profile_default/ipython_config.py`
```
c = get_config()
c.InteractiveShellApp.matplotlib = "inline"
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

