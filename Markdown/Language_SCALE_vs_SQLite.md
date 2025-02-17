# SCALE vs SQLite
SCALE is essentially a superset of [SQLite](https://sqlite.org) syntax. If you are familiar with TSQL, you pretty much already know SCALE. In SCALE, you are basically either calling 1E client methods and modules, or you're executing SQLite select statements. Its important to know where SQLite syntax rules are followed and where SCALE rules are followed.

## Visual example
Everything from the word `SELECT` all the way to the semi-colon `;` at the end is ultimately sent to SQLite.  The rest is SCALE.  

You need to follow the rules of SCALE when in the SCALE part of the code, and SQLite rules when in the SQLite part of the code.
```
                                                                      /*
     SCALE
╔═════════════╗                                                       */
//get processes
@procs = OperatingSystem.GetProcesses();                              /*
╚══════════════════════════════════════╝
                  SCALE                  

                                     SQLite
               ╔════════════════════════════════════════════════════╗ */
@chromeProcs = SELECT * FROM @procs WHERE Executable LIKE "%chrome%"; /*
╚═════════════╝
     SCALE                                                            */
```

## Calling methods and comments = SCALE
When executing the 1E client methods, or making comments you need to follow the syntax rules of SCALE.

```
//ALL SCALE  
//get information about the device
@dvc = Device.GetSummary();
```
Comments, assigning data to variables, calling methods, line ending semi-colons, these all follow SCALE syntax rules like case-sensitive object names, column names, parameter names, etc.

---

## SELECT statements = SQLite
Everything between a SELECT and the closing semi-colon `;` is passed to the SQLite engine. This means all SQLite syntax rules are used, other than table names all start with `@` in our version of SQLite.

```
//SCALE COMMENTS AND METHODS
//get processes
@procs = OperatingSystem.GetProcesses();

//SQLite from SELECT to semi-colon ;
@chromeProcs = SELECT * FROM @procs WHERE Executable like "%chrome%";
```
Everything from the word `SELECT` all the way to the semi-colon `;` at the end is ultimately sent to SQLite.  The rest is SCALE.

---
