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

---

### Setting options

#### For the entire session
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

