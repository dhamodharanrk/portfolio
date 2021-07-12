---
title: Rename Pandas DataFrame Index
tags: [Python,Pandas]
style: border
color: secondary
description: "Rename Pandas DataFrame Index"
---

Pandas has some quirkiness when it comes to renaming the levels of the index. There is also a new DataFrame method rename_axis available to change the index level names.

Let's take a look at a DataFrame

    df = pd.DataFrame({'age':[30, 2, 12],
                           'color':['blue', 'green', 'red'],
                           'food':['Steak', 'Lamb', 'Mango'],
                           'height':[165, 70, 120],
                           'score':[4.6, 8.3, 9.0],
                           'state':['NY', 'TX', 'FL']},
                           index = ['Jane', 'Nick', 'Aaron'])
						   

This DataFrame has one level for each of the row and column indexes. Both the row and column index have no name. Let's change the row index level name to 'names'.

`df.rename_axis('names')`

The rename_axis method also has the ability to change the column level names by changing the axis parameter:

`df.rename_axis('names').rename_axis('attributes', axis='columns')`

If you set the index with some of the columns, then the column name will become the new index level name. Let's append to index levels to our original DataFrame:

`df1 = df.set_index(['state', 'color'], append=True)`

Notice how the original index has no name. We can still use rename_axis but need to pass it a list the same length as the number of index levels.

`df1.rename_axis(['names', None, 'Colors'])`

You can use None to effectively delete the index level names.

Series work similarly but with some differences
Let's create a Series with three index levels

`s = df.set_index(['state', 'color'], append=True)['food']`

We can use rename_axis similarly to how we did with DataFrames

`s.rename_axis(['Names','States','Colors'])`

Notice that the there is an extra piece of metadata below the Series called Name. When creating a Series from a DataFrame, this attribute is set to the column name.

We can pass a string name to the rename method to change it

`s.rename('FOOOOOD')`

DataFrames do not have this attribute and infact will raise an exception if used like this

`df.rename('my dataframe')`

TypeError: 'str' object is not callable

Prior to pandas 0.21, you could have used rename_axis to rename the values in the index and columns. It has been deprecated so don't do this

[Rename-Pandas-Dataframe-Index][1]
[1]: https://stackoverflow.com/questions/19851005/rename-pandas-dataframe-index "rename-pandas-dataframe-index"
