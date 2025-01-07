# FOREACH Statement
The `FOREACH` statement is a control-of-flow structure that allows you to loop through each row of a table and operate on each row independently from each other.

The output from a `FOREACH` statement can optionally be assigned to a @table variable

## FOREACH Syntax
```
                                                      /*
   Assignment Optional
╔═════════════════════════╗                           */
@optional_table_variable =
  FOREACH @row IN @table DO
    //code to execute for each row
  DONE;
```
This will loop through the table `@table` and for each row of that table, put that row's contents into a single-row in-memory table called `@row`.
That table can be referenced in between the `FOREACH` and `DONE`

#### FOREACH example
In the following code, we get all processes, get a list of distinct processIDs for processes named something like `msedge%.exe` and use a `FOREACH` loop to loop through each of them and issue a kill command against each ID
```
//get all currently-running procs
@procs = OperatingSystem.GetProcesses();

//find any related to MSedge/MSEdgeWebview/etc. 
@edges = SELECT DISTINCT ProcessId FROM @procs WHERE Executable LIKE "msedge%.exe";

//loop through processes and kill each one
FOREACH @proc IN @edges DO
    //first convert the ProcessId to TEXT because KillProcess requires the ID to be a string
    @proc = SELECT CAST(ProcessId AS TEXT) ProcessId FROM @proc;

    //and kill the process using it's ID
    OperatingSystem.KillProcess(ProcessId:@proc.ProcessId);
ENDIF;
```

#### Returning data from a FOREACH loop
The output from the last statement in a `FOREACH` loop can be captured in a table by simply adding a table assignment to the beginning of it.

This is the same code as above, but we've added a little extra at the end of the `FOREACH` to return more info, and assigned the loop to a `@result` table
```
//get all currently-running procs
@procs = OperatingSystem.GetProcesses();

//find any related to MSedge/MSEdgeWebview/etc. 
@edges = SELECT DISTINCT ProcessId, Executable FROM @procs WHERE Executable LIKE "msedge%.exe";

//loop through processes and kill each one
//assign the output from the last command in the FOREACH statement into a table called @result
@result = 
  FOREACH @proc IN @edges DO
      //first convert the ProcessId to TEXT because KillProcess requires the ID to be a string
      @proc = SELECT CAST(ProcessId AS TEXT) ProcessId, Executable FROM @proc;
  
      //and kill the process using it's ID
      @killResult = OperatingSystem.KillProcess(ProcessId:@proc.ProcessId);

      //return the ProcessId and Executable from the @proc table and Killed from the @killResult table
      SELECT
        proc.ProcessId,
        proc.Executable,
        kr.Killed
      FROM
        @proc proc
        CROSS JOIN @killResult kr;
  DONE;

//now we've got a @result table with all of the PIDs and their termination status
SELECT * FROM @result;
```
