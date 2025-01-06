# Data Types
Similar to [comments](./Language_Comments.md) we need to understand when we're in SCALE and when we're in SQLite to understand what data types we're working with.

The basic rules for data types are:
* SCALE has only in-memory SQLite tables
* During SQLite processing, the columns can have `NULL`, `INTEGER`, `REAL`, `TEXT` and `BLOB` data types (See [LINK](https://sqlite.org/datatype3.html))
* After an instruction or automation has run, the data that gets returned can be one of the following<br>![image](https://github.com/user-attachments/assets/acca0671-eabb-408e-9d95-e26c5395f4e8)

For more information about COLUMN data types returned from an instruction, see [TIMS_Schema](./TIMS_Schema.md)

## SCALE only deals in tables
When we run a SCALE command like 
```
@device = Device.GetSummary();
```
the result is an in-memory SQLite table called `@device`.
All `@` variables will always be the data type of "table".  Even if there's only one column and one row, and even if it's an empty string with no rows, all variables are tables.

## No data types in SCALE
Although you can assign a value to a variable like this
```c
@myNum = 1;
```
that statement is just shorthand for 
```c
@myNum = SELECT 1 AS [Value];
```
  
The `@myNum` variable is still an in-memory SQLite table with one column and one row.  

You can see this by running that statement by itself  
  
![image](https://github.com/user-attachments/assets/44ace5e6-ad6d-4e45-8e65-342df895c9e4)

Notice that there's one column called Value and has one row with that number 1 in it.
So although it looked like a scalar/integer value, it's still a table that you can select from later on.

## Data types in SQLite table columns
As seen in this [LINK](https://sqlite.org/datatype3.html), the SQLite table columns can be one of `NULL`, `INTEGER`, `REAL`, `TEXT` and `BLOB` data types.  

If you are familiar with Microsoft SQL Server or other enterprise relational database engines, you may be used to seeing types like `VARCHAR`, `NVARCHAR`, `TINYINT`, `SMALLINT`, `NCHAR`, `BOOLEAN`, `DECIMAL`, etc. but SQLite is much simpler and only has the 5 types mentioned.

## Use `CAST` to change from one SQLite data type to another
If you're going to write a query in your code that may output a mix of datatypes for the same column, you'll want to use the `CAST` function to bring them into alignment  

In the following code, the `ProcessorTimePercent` column may have an `INTEGER` data type for some rows (like `0`) or it may have a `REAL` data type for other rows (like `0.4223`).  
```c
@dta = SELECT * FROM $SoftwarePerformance_Live WHERE Product LIKE "%Edge WebView%";
SELECT
    Product,
    SUM(CAST(ProcessorTimePercent AS REAL)) AS TotalProcessorTime
FROM
    @dta
GROUP BY
    Product
ORDER BY
    TotalProcessorTime DESC
LIMIT 10;
``` 
By using `CAST(ProcessorTimePercent AS REAL)` we are essentially taking any `NULL`, `INTEGER`, `TEXT` or `BLOB` data that may be in that column and converting them to `REAL` so that the `SUM()` operation doesn't fail or perform integer math instead of floating point math which could give you the wrong numbers.
