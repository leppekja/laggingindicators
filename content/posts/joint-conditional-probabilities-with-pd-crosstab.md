---
title: "Joint &amp; conditional probabilities with pd.crosstab"
date: "2024-05-02"
tags: 
  - "analytics"
  - "coding"
---

`pd.crosstab` is one of those built-in functions in the Pandas API that I forget about routinely. I instinctively reached for `df.groupby('x')['y'].count().unstack()`, but when I wanted to normalize the values, it takes [more and more steps](https://stackoverflow.com/questions/37818063/how-to-calculate-conditional-probability-of-values-in-dataframe-pandas-python) to get where I wanted.

[This was a nice straightforward overview](https://lisds.github.io/textbook/useful-pandas/crosstab.html) of the `pd.crosstab` function. To document for myself, below, create a sample correlated `DataFrame` with integer columns `ActiveUsers` and `CompletedProfile`.

```python
import pandas as pd
import numpy as np

# following code from Github Copilot
np.random.seed(0)
n = 1000  # Number of samples
p = 0.7  # Probability of True in the first column
rho = 0.8  # Correlation

col1 = np.random.choice([True, False], size=n, p=[p, 1-p])
col2 = np.where(col1, np.random.choice([True, False], size=n, p=[rho, 1-rho]), 
         np.random.choice([True, False], size=n, p=[1-rho, rho]))

df = pd.DataFrame({'ActiveUsers': col1, 'CompletedProfile': col2})
```

<figure>

<table class="has-fixed-layout"><tbody><tr><td></td><td>ActiveUsers</td><td>CompletedProfile</td></tr><tr><td>0</td><td>True</td><td>True</td></tr><tr><td>1</td><td>False</td><td>False</td></tr><tr><td>2</td><td>True</td><td>False</td></tr><tr><td>3</td><td>...</td><td>...</td></tr></tbody></table>

<figcaption>

Sample of the constructed DataFrame

</figcaption>



</figure>

Running `pd.crosstab(df['ActiveUsers'], df['CompletedProfile'])` gets a frequency distribution. Passing `normalize=True` adjusts the numbers of each group relative to the sum of the groups.

With `normalize='index'`, we can find conditional probabilities given the row values. Given the value in the index, we can see the relative frequencies in the columns; numbers are normalized according to total values of each row. This gives us P(CompletedProfile=Y|ActiveUser=X).

<figure>

<table class="has-fixed-layout"><tbody><tr><td>CompletedProfile</td><td>False</td><td>True</td></tr><tr><td>ActiveUsers</td><td></td><td></td></tr><tr><td>False</td><td>.797</td><td>.202</td></tr><tr><td>True</td><td>.220</td><td>.779</td></tr></tbody></table>

<figcaption>

Result of pd.crosstab(df\['ActiveUsers'\], df\['CompletedProfile'\], normalize='index'). Given a ActiveUser value of True, ~78% of instances are CompletedProfile=True.

</figcaption>



</figure>

With `normalize='columns'`, the conditional probabilities are based on the columns, so we find P(ActiveUser=Y|CompletedProfile=X). Given a value in a column, what is the relative frequency of each row value.

<figure>

<table class="has-fixed-layout"><tbody><tr><td>CompletedProfile</td><td>False</td><td>True</td></tr><tr><td>ActiveUsers</td><td></td><td></td></tr><tr><td>False</td><td>.597</td><td>.096</td></tr><tr><td>True</td><td>.402</td><td>.903</td></tr></tbody></table>

<figcaption>

Result of pd.crosstab(df\['ActiveUsers'\], df\['CompletedProfile'\], normalize='columns'). Given a CompletedProfile value of True, ~90% of instances are ActiveUsers=True.

</figcaption>



</figure>

Just so I don't forget this:

<figure>

<table class="has-fixed-layout"><tbody><tr><td>pd.crosstab with normalize arg</td><td>Resulting conditional probability</td></tr><tr><td>pd.crosstab(df.ColumnA, df.ColumnB, normalize='index')</td><td>P(ColumnB|ColumnA)</td></tr><tr><td>pd.crosstab(df.ColumnA, df.ColumnB, normalize='columns')</td><td>P(ColumnA|ColumnB)</td></tr></tbody></table>

<figcaption>

Resulting conditional probabilities by normalize arg.

</figcaption>



</figure>

Curiously, passing `margins=True` when normalizing by `index` or `columns` gives totals (normalized) for the columns or rows, respectively. The Pandas documentation doesn't cover this, but it is strange that the `margins=True` doesn't result in a totals relative to the values being normalized against.

<figure>

<table class="has-fixed-layout"><tbody><tr><td>CompletedProfile</td><td>False</td><td>True</td><td>All</td></tr><tr><td>ActiveUsers</td><td></td><td></td><td></td></tr><tr><td>False</td><td>.597</td><td>.096</td><td>.291</td></tr><tr><td>True</td><td>.402</td><td>.903</td><td>.709</td></tr></tbody></table>

<figcaption>

Result of pd.crosstab(df\['ActiveUsers'\], df\['CompletedProfile'\], margins=True, normalize='columns'). Values in the All column just reflect the normalized totals relative to the entire table, as if you ran pd.crosstab(df\['ActiveUsers'\], df\['CompletedProfile'\], normalize=True, margins=True).

</figcaption>



</figure>
