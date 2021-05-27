Making and filling
---------------------

From list of dicts:

```
rows = []
indces = []

for ....
   rows.append({'col':1,'col2':2})
   indices.append(r'row {i}')
   
df = pd.DataFrame(rows, index=indices)


```

Indexing:

element = table.at['a','b']

adding an element

table.at['col','row'] = value

adding a column:

table['new column name'] = 0  # undefined contents

adding a row

frame.loc['row1'] = 0

Adding a summary row

frame.loc['max'] = frame[frame.columns].max()

Selecting rows / columns by index
----------------------------------

.. code-block:: py

    row = frame.iloc[irow]
    col = frame.iloc[:, icol]



Multiindex 
-----------

A MutliIndex has multiple "levels"

.. code-block:: py

   arrays = [[1, 1, 2, 2], ['red', 'blue', 'red', 'blue']]
   index = pd.MultiIndex.from_arrays(arrays, names=('number', 'color'))
   MultiIndex([(1,  'red'),
            (1, 'blue'),
            (2,  'red'),
            (2, 'blue')],
           names=['number', 'color'])
           
  data = pd.DataFrame([[1, 2, 3, 4]], columns=index)

Selecting

- Names of the levels: index.names
- Get values in index.get_level_values(0)
- Get all levels and all values : index.levels

- Select from dataframe using [ ], here : my be used to denote any value. Example
- ```data.loc['Value level 0', : 'Value level 2']``` . Trailing : my be omitted
- ```data.query("color == 'red' and number == 2)```


Filtering on values
---------------------
- ```frame = frame[frame['column'] > 20]```
- ```frame = frame[frame['column'] > 20 & frame['column'] < 40]```

- And when a multi-index is used for the column names:``` test[test[('GlobalForce','X')] > 0]```


Filtering on index range (times)
---------------------------------

```start = pd.Timestamp('2021-05-23 06:55:00')
end = pd.Timestamp('2021-05-23 07:10:00')
section = df[(df.index>start) & (df.index<end)]
```
