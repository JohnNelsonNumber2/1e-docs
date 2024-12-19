# Data Types
Similar to [comments](./Language_Comments.md) we need to understand when we're in SCALE and when we're in SQLite to understand what data types we're working with.

## SCALE only deals in tables
When we run a SCALE command like 
```
@device = Device.GetSummary();
```
the result gets put into an in-memory SQLite table called `@device`. All `@` variables will always be the data type of "table".  Even if there's only one column and one row or if there are no rows, all variables are tables.



## Tables in SQLite
