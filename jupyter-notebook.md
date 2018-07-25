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

