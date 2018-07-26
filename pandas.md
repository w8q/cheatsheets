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

---

```python
d1 = dict(a=[1,2,3,4,5,6], b=list('abcdef'))
d2 = dict(a=[1,2,5,7,8,9], c=[True, False, True, True, True, False])
df1 = pd.DataFrame(d1)
df2 = pd.DataFrame(d2)
df1
df2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>c</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>d</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>e</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>f</td>
    </tr>
  </tbody>
</table>
</div>


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>


```python
pd.merge(df1, df2, on='a', how='left')
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>c</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>d</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>e</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>f</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


```python
pd.merge(df1, df2, on='a', how='right')
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>e</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>


```python
pd.merge(df1, df2, on='a', how='inner')
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>e</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>


```python
pd.merge(df1, df2, on='a', how='outer')
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>c</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>d</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>e</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>f</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>

