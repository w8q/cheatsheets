<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [`groupby` without aggregation](#groupby-without-aggregation)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---

### `groupby` without aggregation

```python
import numpy as np
import pandas as pd

nrow, ncol = 12, 5
df = pd.DataFrame(np.random.randint(1, 1000, nrow*ncol).reshape(nrow, ncol))

categories = pd.Series(np.random.choice(list('abc'), nrow), name='Cat')

df = pd.concat([categories, df], axis=1)
print(df)
```

```sh
   Cat    0    1    2    3    4
0    c  872  937  837  229  905
1    a  362  493  952  976  516
2    b  472  577  407  134  436
3    b  272  307  590  767  549
4    c  605  547  594  381  171
5    c  635  863  978  836  797
6    a  582  884  865  351  350
7    c  515  294  227  346  696
8    c  900  759  369  304  968
9    c  223  711  842  716  101
10   c   86  293  785  559  757
11   c  192   28  465   35  559
```


```python
by_cat = df.groupby('Cat').agg(list)
print(by_cat[3])
```

```sh
Cat
a                                 [976, 351]
b                                 [134, 767]
c    [229, 381, 836, 346, 304, 716, 559, 35]
Name: 3, dtype: object
```

