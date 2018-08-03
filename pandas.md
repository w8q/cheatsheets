### Setting options

#### For the entire session
https://pandas.pydata.org/pandas-docs/stable/options.html
```python
>>> import pandas as pd

# None = unlimited
>>> pd.set_option('max_rows', None)
>>> pd.set_option('max_columns', None)
```

#### Using context
https://pandas.pydata.org/pandas-docs/stable/generated/pandas.option_context.html
```python
>>> import numpy as np
>>> import pandas as pd

>>> nrow, ncol = 20, 30
>>> df = pd.DataFrame(np.random.randint(1, 1000, nrow*ncol).reshape(nrow, ncol))

>>> with pd.option_context('display.max_rows', 100,
                           'display.max_columns', 3):
        print(df)
```
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

---

### `groupby` without aggregation

- Dataframe setup

```python
>>> import numpy as np
>>> import pandas as pd

>>> nrow, ncol = 12, 5
>>> df = pd.DataFrame(np.random.randint(1, 1000, nrow*ncol).reshape(nrow, ncol))

>>> categories = pd.Series(np.random.choice(list('abc'), nrow), name='Cat')

>>> df = pd.concat([categories, df], axis=1)
>>> print(df)
```
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

- trick: `.agg(set)` or `.agg(list)`


```python
>>> by_cat = df.groupby('Cat').agg(list)
>>> print(by_cat[3])
```
    Cat
    a                                 [976, 351]
    b                                 [134, 767]
    c    [229, 381, 836, 346, 304, 716, 559, 35]
    Name: 3, dtype: object

---

### Simple `merge`

- Dataframe setup

```python
>>> d1 = dict(a=[1,2,3,4,5,6], b=list('abcdef'))
>>> d2 = dict(a=[1,2,5,7,8,9], c=[True, False, True, True, True, False])
>>> df1 = pd.DataFrame(d1)
>>> df2 = pd.DataFrame(d2)
>>> print(df1)
>>> print(df2)
```

       a  b
    0  1  a
    1  2  b
    2  3  c
    3  4  d
    4  5  e
    5  6  f

       a      c
    0  1   True
    1  2  False
    2  5   True
    3  7   True
    4  8   True
    5  9  False


- `left` join

```python
>>> print(pd.merge(df1, df2, on='a', how='left'))
```

       a  b      c
    0  1  a   True
    1  2  b  False
    2  3  c    NaN
    3  4  d    NaN
    4  5  e   True
    5  6  f    NaN


- `right` join

```python
>>> print(pd.merge(df1, df2, on='a', how='right'))
```

       a    b      c
    0  1    a   True
    1  2    b  False
    2  5    e   True
    3  7  NaN   True
    4  8  NaN   True
    5  9  NaN  False


- `inner` join

```python
>>> print(pd.merge(df1, df2, on='a', how='inner'))
```

       a  b      c
    0  1  a   True
    1  2  b  False
    2  5  e   True


- `outer` join

```python
>>> print(pd.merge(df1, df2, on='a', how='outer'))
```
       a    b      c
    0  1    a   True
    1  2    b  False
    2  3    c    NaN
    3  4    d    NaN
    4  5    e   True
    5  6    f    NaN
    6  7  NaN   True
    7  8  NaN   True
    8  9  NaN  False

---

### One-hot and Label Encoding

- Dataframe setup

```python
>>> import numpy as np
>>> import pandas as pd

>>> nrow, ncol = 12, 5
>>> df = pd.DataFrame(np.random.randint(1, 1000, nrow*ncol).reshape(nrow, ncol))

>>> categories = pd.Series(np.random.choice(list('abc'), nrow), name='Cat')

>>> df = pd.concat([df, categories], axis=1)
>>> print(df)
```

          0    1    2    3    4 Cat
    0   420  805  857  731  883   b
    1   157   36  892  780  749   a
    2   377  501  333  999  549   a
    3   846  899  636   33  236   a
    4   619  760  710  732  528   c
    5   593  885   40  460  582   b
    6   187  740  996  919  723   a
    7   320  844  246  903  500   a
    8   132  446  607   67  415   a
    9    18  466  975  378  730   a
    10  507  105  784  636  286   c
    11  265  307  483  360  467   c


#### Label Encode using `pd.factorize()`

- https://pandas.pydata.org/pandas-docs/stable/generated/pandas.factorize.html

```python
>>> labels, uniques = pd.factorize(df['Cat'])

>>> labels
array([0, 1, 1, 1, 2, 0, 1, 1, 1, 1, 2, 2])

>>> uniques
Index(['b', 'a', 'c'], dtype='object')

>>> uniques.take(labels)
Index(['b', 'a', 'a', 'a', 'c', 'b', 'a', 'a', 'a', 'a', 'c', 'c'], dtype='object')

>>> df['Cat'].values
array(['b', 'a', 'a', 'a', 'c', 'b', 'a', 'a', 'a', 'a', 'c', 'c'],
      dtype=object)

>>> pd.concat([df, pd.Series(labels, name='Cat_label')], axis=1)
          0    1    2    3    4 Cat  Cat_label
    0   420  805  857  731  883   b          0
    1   157   36  892  780  749   a          1
    2   377  501  333  999  549   a          1
    3   846  899  636   33  236   a          1
    4   619  760  710  732  528   c          2
    5   593  885   40  460  582   b          0
    6   187  740  996  919  723   a          1
    7   320  844  246  903  500   a          1
    8   132  446  607   67  415   a          1
    9    18  466  975  378  730   a          1
    10  507  105  784  636  286   c          2
    11  265  307  483  360  467   c          2
```

#### One-hot encode using `pd.get_dummies()`

- https://pandas.pydata.org/pandas-docs/stable/generated/pandas.get_dummies.html

```python
>>> oh_cat = pd.get_dummies(df['Cat'], prefix='Cat')

>>> oh_cat
    Cat_a  Cat_b  Cat_c
0       0      1      0
1       0      0      1
2       1      0      0
3       1      0      0
4       0      0      1
5       1      0      0
6       1      0      0
7       1      0      0
8       0      0      1
9       0      1      0
10      0      1      0
11      1      0      0

>>> pd.concat([df, oh_cat], axis=1)
      0    1    2    3    4 Cat  Cat_a  Cat_b  Cat_c
0   243  664   99   79  389   b      0      1      0
1   927  680  768  256  702   c      0      0      1
2   705   30  148  840  262   a      1      0      0
3   653  677  930  531  389   a      1      0      0
4   239  730  561  303    8   c      0      0      1
5    57  130  953  302  758   a      1      0      0
6   355  635   10  776  658   a      1      0      0
7   981  317  230  832  309   a      1      0      0
8    45  124  508  109  710   c      0      0      1
9   875  820  134  775  269   b      0      1      0
10  248  860  867  690  438   b      0      1      0
11   15  342  913  631  128   a      1      0      0
```

---

### `groupby('col').count()` vs `grouby('col').size()`

`size` includes `NaN` values, `count` does not. See [stackoverflow](https://stackoverflow.com/a/33346694).

```python
>>> import numpy as np
>>> import pandas as pd

>>> nrow, ncol = 20, 2
>>> df = pd.DataFrame(np.random.randint(1, 5, nrow*ncol).astype('uint32').reshape(nrow, ncol))
>>> df.iloc[2:5,0] = np.nan
>>> print(df)
      0  1
0   1.0  2
1   3.0  3
2   NaN  3
3   NaN  1
4   NaN  1
5   2.0  2
6   1.0  2
7   1.0  1
8   4.0  3
9   4.0  3
10  2.0  2
11  4.0  4
12  1.0  3
13  1.0  4
14  3.0  3
15  3.0  3
16  1.0  2
17  4.0  3
18  2.0  4
19  4.0  2
>>> print(df.groupby(1).count())
   0
1
1  1
2  6
3  7
4  3
>>> print(df.groupby(1).size())
1
1    3
2    6
3    8
4    3
dtype: int64
```
