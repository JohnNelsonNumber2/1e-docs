# IF Statement
The `IF` statement is a control-of-flow structure that allows you to execute code only if some condition is met, and optionally if some condition is not met.

The output from an `IF` statement can optionally be assigned to a @table variable

## IF Syntax
With the basic syntax, you specify only the name of a table inside the `IF()` parentheses.
```
                                                      /*
   Assignment Optional
╔═════════════════════════╗                           */
@optional_table_variable =
  IF(@there_are_rows_in_this_table)
    //code to execute if table has rows
  ELSE
    //optional code to execute if table has no rows
  ENDIF;
```
Notice that you do not put any operators inside the parenthesis to check for equality.  Only the name of a table.
 
In other words, `IF(@windows = True)` is not a valid statement because the `IF` statement requires the name of a table only, no operators.  

#### Example IF
If you need to perform some equality check on a value or look for the existence of some value in a table, you have to create a table first with a SELECT statement that has all your logic in it.
```
//get info about the device, including the OS
@device = Device.GetSummary();

//determine if this is windows by using a WHERE clause on that device table
@windows = SELECT OperatingSystemType FROM @device WHERE OperatingSystemType = "Windows";

//if there were rows in the @windows table, we are running Windows so execute Windows code
IF(@windows)
  //execute code appropriate for Windows OS
ENDIF;
```
 
#### Example IF ELSE
It's not necessary, but you may also include an ELSE statement to execute code if some condition is NOT met.
```
//get info about the device, including the OS
@device = Device.GetSummary();

//determine if this is windows by using a WHERE clause on that device table
@windows = SELECT OperatingSystemType FROM @device WHERE OperatingSystemType = "Windows";

//if there were rows in the @windows table, we are running Windows so execute Windows code
IF(@windows)
  //execute code appropriate for Windows OS
ELSE
  //execute code appropriate for Non-Windows (MacOS, Linux, etc.)
ENDIF;
```

#### Optional NOT keyword
It is also possible to use a `NOT` keyword in front of the table in order to execute code if there are NOT rows in that table.
```
//get info about the device, including the OS
@device = Device.GetSummary();

//determine if this is windows by using a WHERE clause on that device table
@windows = SELECT OperatingSystemType FROM @device WHERE OperatingSystemType = "Windows";

//if there are NOT rows in the Windows table, exit with a NOTIMPLEMENTED status
IF(@windows)
    NOTIMPLEMENTED "This instruction runs on Windows only";
ENDIF;

//otherwise continue with the instruction
```

#### Returning data from the IF statement
The output from the last command inside an IF block can be captured in a table by simply adding a table assignment to the beginning of it.

```
//get info about the device, including the OS
@device = Device.GetSummary();

//determine if this is windows by using a WHERE clause on that device table
@windows = SELECT OperatingSystemType FROM @device WHERE OperatingSystemType = "Windows";

//if there were rows in the @windows table, we are running Windows so execute Windows code
//assign the output from the last command in the IF statement into a table called @result
@result = 
  IF(@windows)
    OperatingSystem.RunCommand(CommandLine:"powercfg /query");
  ENDIF;

//there is now a @result table with the output of that powercfg statement
SELECT * FROM @result;
```
