# Filtering Pandas and DataFrames

Remember the brics.csv file from earlier:

```csv
,country,capital,area,population
BR,Brazil,Brasilia,8.516,200.4
RU,Russia,Moscow,17.10,143.5
IN,India,New Delhi,3.286,1252
CH,China,Beijing,9.597,1357
SA,South Africa,Pretoria,1.221,52.98
```

We've already used `pandas` to create DataFrames from csv files, but now let's integrate the comparison and boolean operators we've learned into this data type. Let's get the data frame to work with first:

```py
import pandas as pd

brics = pd.read_csv("brics.csv", index_col=0)

print(brics)

#          country    capital    area  population
# BR        Brazil   Brasilia   8.516      200.40
# RU        Russia     Moscow  17.100      143.50
# IN         India  New Delhi   3.286     1252.00
# CH         China    Beijing   9.597     1357.00
# SA  South Africa   Pretoria   1.221       52.98
```

If we want to get just the countries with an area greater than 8 million square kilometers, we have to take three steps.

## Filtering

### Get Column

The first step is to get the column with the data we want to filter by. There are a handful of ways to go about this, but in this case, we want to get a Series, not a DataFrame, so make sure to use single brackets instead of double brackets.

```py
brics["area"]

# or

brics.loc[:, "area"]

# or

brics.iloc[:, 2]

# BR     8.516
# RU    17.100
# IN     3.286
# CH     9.597
# SA     1.221
# Name: area, dtype: float64
```

But we don't need all of the areas, we just need to ones bigger than eight.

### Compare

We can add a comparison operator to our Series to convert each of the values to a boolean, in this case, "greater than eight".

```py
brics.loc[:, "area"] > 8

# BR     True
# RU     True
# IN    False
# CH     True
# SA    False
# Name: area, dtype: bool
```

So now we have a collection of countries where the area is greater than 8 million square kilometers, represented by a True value.

### Subset

Finally, we can take the value from step two and plug it directly into our brics object:

```py
brics[brics.loc[:, "area"] > 8]

#    country   capital    area  population
# BR  Brazil  Brasilia   8.516       200.4
# RU  Russia    Moscow  17.100       143.5
# CH   China   Beijing   9.597      1357.0
```

All together, Step One - Get Column, determines which value you want to reference when you filter, Step Two - Compare, determines the means by which you filter that column, and Step Three - Subset, plugs that filtering into the object. This makes it so you determine which of your rows are given a True value, and you return all the original rows where that value is true:

```py
brics.loc[:, "area" > 8]

# BR     True
# RU     True
# IN    False
# CH     True
# SA    False
# Name: area, dtype: bool

# BR, RU, and CH are all True

brics[brics.loc[:, "area"] > 8]

#    country   capital    area  population
# BR  Brazil  Brasilia   8.516       200.4
# RU  Russia    Moscow  17.100       143.5
# CH   China   Beijing   9.597      1357.0

# BR, RU, and CH are the rows that get returned
```

## Boolean Operators

But if we want to check for more than one parameter, like area AND population, we need to use boolean operators. To do this, we can import `numpy` as well, and we now have access to our logical_and(), logical_or() and logical_not() funtions, all of which can be applied to your DataFrame data.

```py
import numpy as np

brics[np.logical_and(brics["area"] > 8, brics["population"] > 1000)]

#    country  capital   area  population
# CH   China  Beijing  9.597      1357.0
```

Now we can search for the countries that have both an area greater than 8 million square kilometers, and a population greater than one billion, in this case, just China.
