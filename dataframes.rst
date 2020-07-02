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


Multiindex 
-----------


            arrays = [[1, 1, 2, 2], ['red', 'blue', 'red', 'blue']]
            pd.MultiIndex.from_arrays(arrays, names=('number', 'color'))
            MultiIndex([(1,  'red'),
                        (1, 'blue'),
                        (2,  'red'),
                        (2, 'blue')],
                       names=['number', 'color'])
