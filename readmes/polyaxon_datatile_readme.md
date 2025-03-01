[![License: Apache 2](https://img.shields.io/badge/License-apache2-green.svg)](LICENSE)
[![Datatile](https://github.com/polyaxon/datatile/actions/workflows/datatile.yml/badge.svg)](https://github.com/polyaxon/datatile/actions/workflows/datatile.yml)
[![Slack](https://img.shields.io/badge/chat-on%20slack-aadada.svg?logo=slack&longCache=true)](https://polyaxon.com/slack/)
[![Docs](https://img.shields.io/badge/docs-stable-brightgreen.svg?style=flat)](https://polyaxon.com/docs/)
[![GitHub](https://img.shields.io/badge/issue_tracker-github-blue?logo=github)](https://github.com/polyaxon/datatile/issues)
[![GitHub](https://img.shields.io/badge/roadmap-github-blue?logo=github)](https://github.com/polyaxon/)

<a href="https://polyaxon.com"><img src="https://raw.githubusercontent.com/polyaxon/polyaxon/master/artifacts/packages/datatile.svg" width="125" height="125" align="right" /></a>

# Datatile

A library for managing, summarizing, and visualizing data.

> **N.B.1**: `pandas-summary` was renamed to datatile, a more ambitious project with sevral planned features and enhancements to add support for visualizations, quality checks, linking summaries to versions, and integrations with third party libraries.

## Installation

The module can be easily installed with pip:

```conslole
> pip install datatile
```

This module depends on `numpy` and `pandas`. Optionally you can get also some nice visualisations if you have `matplotlib` installed.

## Tests

To run the tests, execute the command `python setup.py test`

## Usage

### DataFrameSummary

An extension to [pandas](http://pandas.pydata.org/) dataframes describe function.

The module contains `DataFrameSummary` object that extend `describe()` with:

- **properties**
  - dfs.columns_stats: counts, uniques, missing, missing_perc, and type per column
  - dsf.columns_types: a count of the types of columns
  - dfs[column]: more in depth summary of the column
- **function**
  - summary(): extends the `describe()` function with the values with `columns_stats`

The `DataFrameSummary` expect a pandas `DataFrame` to summarise.

```python
from datatile.summary.df import DataFrameSummary

dfs = DataFrameSummary(df)
```

getting the columns types

```python
dfs.columns_types


numeric     9
bool        3
categorical 2
unique      1
date        1
constant    1
dtype: int64
```

getting the columns stats

```python
dfs.columns_stats


                      A            B        C              D              E
counts             5802         5794     5781           5781           4617
uniques            5802            3     5771            128            121
missing               0            8       21             21           1185
missing_perc         0%        0.14%    0.36%          0.36%         20.42%
types            unique  categorical  numeric        numeric        numeric
```

getting a single column summary, e.g. numerical column

```python
# we can also access the column using numbers A[1]
dfs['A']

std                                                                 0.2827146
max                                                                  1.072792
min                                                                         0
variance                                                           0.07992753
mean                                                                0.5548516
5%                                                                  0.1603367
25%                                                                 0.3199776
50%                                                                 0.4968588
75%                                                                 0.8274732
95%                                                                  1.011255
iqr                                                                 0.5074956
kurtosis                                                            -1.208469
skewness                                                            0.2679559
sum                                                                  3207.597
mad                                                                 0.2459508
cv                                                                  0.5095319
zeros_num                                                                  11
zeros_perc                                                               0,1%
deviating_of_mean                                                          21
deviating_of_mean_perc                                                  0.36%
deviating_of_median                                                        21
deviating_of_median_perc                                                0.36%
top_correlations                         {u'D': 0.702240243124, u'E': -0.663}
counts                                                                   5781
uniques                                                                  5771
missing                                                                    21
missing_perc                                                            0.36%
types                                                                 numeric
Name: A, dtype: object
```

## Future development

### Summaries

 * [ ] Add summary analysis between columns, i.e. `dfs[[1, 2]]`

### Visualizations

 * [ ] Add summary visualization with matplotlib.
 * [ ] Add summary visualization with plotly.
 * [ ] Add summary visualization with altair.
 * [ ] Add predefined profiling.


### Catalog and Versions

 * [ ] Add possibility to persist summary and link to a specific version.
 * [ ] Integrate with quality libraries.
