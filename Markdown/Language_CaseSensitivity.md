# Case sensitivity
In general, SCALE is a case-sensitive language.
However, while executing an instruction, some processing [may be](Markdown/Language_SCALE_vs_SQLite.md) handed off to the SQLite engine to do some of the work.
Therefore, it's important to understand when your SCALE code will be case-sensitive and when it will be case-insensitive.

### Case sensitive: table names
The following SCALE code gets information about the device from the `GetSummary` method of the `Device` module.
```
 LOWER
╔═════╗
@device = Device.GetSummary();
SELECT * FROM @device;
              ╚══════╝
               LOWER
```
Notice that we're writing to a table called `@device` which is all lowercase  
and we're selecting from `@device` which is also all lowercase.  
  
If you run that code, everything works properly
![image](https://github.com/user-attachments/assets/84261418-66d2-46e1-9f15-ba088e3a3567)

  
Now consider the following code
```
 UPPER
╔═════╗
@DEVICE = Device.GetSummary();
SELECT * FROM @device;
              ╚══════╝
               lower
```
If you run that code, it fails with an error <!-- Unknown variable @device at line 2, column 1 -->  
![image](https://github.com/user-attachments/assets/9385607d-b60c-4b94-a651-0d8730f7296c)  
This is because the code writes to a table called `@DEVICE` which is all uppercase  
but selects from `@device` which is all lowercase.  Table names are case-sensitive so it fails.
