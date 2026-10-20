\<.> -> Required Argument(Positional)
\[.] -> Optional Argument(Keyword)
- Popular Python library for data analysis

- Core Objects in Pandas:
	- DataFrame: A table with *entries* and corresponding *values* 
		`pd.DataFrame(<dict(col: str: row: List[str])>)` Row can take any datatype, depends on the type of data to be stored.
		Column Labels take dict.keys(), and corresponding entries take dict.values()	
	    To manually assign row labels(instead of indexing from 0 -> n-1), use optional argument 'index'
		`pd.DataFrame(<dict(col: str: row: List[str])>, [index: List[str]])` 
	- Series: A sequence of values(list)
		`pd.Series(<val: List[int]>)` S
		Similar to DataFrame we can manually assign row labels
		`pd.Series(<val: List[int]>, [index: List[str]], [name: str])`
- Reading Data Files:
	- Generally stored as a csv file
	- `file = pd.read_csv(<filename: str>)` 
	- `file.shape` -> `tuple(records: int, columns: int)` **This is an attribute, not a function**
	- `file.head()` -> returns first 5 records
	- "*The `pd.read_csv()` function is well-endowed, with over 30 optional parameters you can specify. For example, you can see in this dataset that the CSV file has a built-in index, which pandas did not pick up on automatically. To make pandas use that column for the index (instead of creating a new one from scratch), we can specify an `index_col`.*"[^1]???
		- My interpretation: For the case of `file = pd.read_csv(<filename: str>)`,`file.head()` would return a additional column "Unnamed 0" (which is the csv file index??), however `file = pd.read_csv(<filename: str>,[index_col: int])` would return the expected output
- Writing to Data Files:
	- `dataFrame.to_csv('filename.csv')`

## Indexing, Accessing and Assignment:

- Accessing Series from a DataFrame:
	- Using Attributes: `dataFrame.colName`
	- Using properties of Dictionaries: `dataFrame[colName: str]`
- Accessing Specific Elements from a DataFrame: `dataFrame[colName: str][index]`

- Accessing of Data on the basis of its numerical position in the DataFrame:
	- `dataFrame.iloc[rowIndex: int/List[int]]` -> returns the details of the record present at passed `rowIndex` in the DataFrame. Returns columns with its corresponding values for the selected row index
	- `dataFrame.iloc[start:end, colIndex]` -> returns column values(only for the correct column index), from start row index till(not upto) end row index.
- Accessing of Data on the basis of its label name in the DataFrame:
	- `dataFrame.loc[start:end, colName/colIterable: str/List[str]]` -> first argument would tell us the records to be returned(row indices)
	 NOTE: 'end' in this case is **inclusive**.	
	 Reason for the change:  *Remember that loc can index any stdlib type: strings, for example. If we have a DataFrame with index values `Apples, ..., Potatoes, ...`, and we want to select "all the alphabetical fruit choices between Apples and Potatoes", then it's a lot more convenient to index `df.loc['Apples':'Potatoes']` than it is to index something like `df.loc['Apples', 'Potatoet']` (`t` coming after `s` in the alphabet).*[^1]
	- `dataFrame.set_index(newIndexName: int, str)` using this method we can assign column names as indices as well(set new index name to column name).
- Conditional Access:
	- `dataFrame.loc[dataFrame.colName == specificCol: str]`
	- `dataFrame.loc[dataFrame.colName == specificCol: str & (dataFrame.colName >= specificTarget: int)]`
	- In General: `dataFrame.loc[dataFrame.colName * specificCol: str *..]` (* -> logical/ relational operator)
	- Built-In Conditional Selectors:
		- `isin(<List>)` -> `dataFrame.loc[dataFrame.colName.isin(<specificCol/Target: List[str/int]>)]`
		- `isnull()` and `notnull()` 
- Assignment of Data:
	- `dataFrame[colName: str] = element` -> element will be assigned to every value in the specified column
	- `dataFrame[colName: str] = iterableElement` -> corresponding values will be assigned(based on row index and iterable element index) to the specified column


## Summary Functions and Maps:

- Summary Functions:
	- `dataFrame.colName.describe()` -> A *Type Aware* method to display meaningful info about the column
	- `dataFrame.colName.mean()` ->  returns mean of the column
	- `dataFrame.colName.median()` ->  returns median of the column
	- `dataFrame.colName.unique()` -> returns List of unique values
	- `dataFrame.colName.value_counts()` -> returns List of unique values along with their frequency 
- Maps: used to transform data from current format to a different format.
	- `dataFrame.colName.map(lambda <tempVar>: <tempVar> * <constVar>)`, or any other viable notation of "lambda" -> returns List of indices and value
	- `dataFrame.apply(<funcName>, <axis = "columns">)` -> void return, change the DataFrame in place. Function should operate on each row, if axis = "columns"; else if axis = "index" function should operate on column instead.
	- `review_points_mean = reviews.points.mean()                     reviews.points - review_points_mean` Pandas looks at this code and figures out that we must mean to subtract the mean value from every value in the dataset. This would also work for "object" data type; if "-" was changed to a string and  concatenated and the changed data would also be of "object" data type. 
	- 
## Grouping and Sorting:
- grouby()
- agg()
- multiindexing
- sort_values
[^1]: ref. https://www.kaggle.com/code/residentmario/indexing-selecting-assigning
