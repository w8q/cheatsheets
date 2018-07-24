<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Autoreload](#autoreload)
- [Display all results](#display-all-results)
- [`matplotlib`](#matplotlib)
  - [`import` method](#import-method)
  - [Edit config file at `~/.ipython/profile_default/ipython_config.py`](#edit-config-file-at-ipythonprofile_defaultipython_configpy)
- [`nbextensions`](#nbextensions)
- [Setting DataFrame options](#setting-dataframe-options)
  - [For the entire notebook](#for-the-entire-notebook)
  - [Using context](#using-context)

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

---

### Setting DataFrame options

#### For the entire notebook
https://pandas.pydata.org/pandas-docs/stable/options.html
```python
import pandas as pd
pd.set_option('max_colwidth', 800)
pd.options.display.max_rows = None      # no limit
```

#### Using context
https://pandas.pydata.org/pandas-docs/stable/generated/pandas.option_context.html
```python
import numpy as np
import pandas as pd

nrow, ncol = 20, 30
df = pd.DataFrame(np.random.randint(1, 1000, nrow*ncol).reshape(nrow, ncol))
with pd.option_context('display.max_rows', 100,
                       'display.max_columns', 3):
    print(df)
```
```sh
     0  ...    29
0   728 ...   479
1   404 ...   236
2   195 ...   638
3   555 ...   333
4   732 ...   161
5   429 ...   670
6   289 ...   783
7   687 ...   777
8   273 ...   967
9   921 ...   907
10  625 ...   539
11  890 ...   591
12  107 ...   167
13  818 ...   558
14  787 ...   311
15   85 ...   149
16  653 ...   402
17  266 ...   422
18  760 ...   502
19   39 ...   103

[20 rows x 30 columns]
```
