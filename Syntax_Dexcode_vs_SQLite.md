# Dexcode vs SQLite
Dexcode is essentially a superset of SQLite syntax. If you are familiar with TSQL, you pretty much already know dexcode. In dexcode, you are basically either calling 1E client methods and modules, or you're executing SQLite select statements. Its important to know where SQLite syntax rules are followed and where dexcode rules are followed.

## Calling methods = Dexcode
When executing the 1E client methods, or making comments you need to follow the syntax rules of dexcode.

```c
//ALL DEXCODE  
//get information about the device
@dvc = Device.GetSummary();
```
Comments, assigning data to variables, calling methods, line ending semi-colons, these all follow dexcode syntax rules like case-sensitive object names, column names, parameter names, etc.

---

## SELECT statements = SQLite
Everything between a SELECT and the closing semi-colon `;` is passed to the SQLite engine. This means all SQLite syntax rules are used, other than table names all start with `@`

```c
//DEXCODE COMMENTS AND METHODS
//get processes
@procs = OperatingSystem.GetProcesses();

//SQLite from SELECT to semi-colon ;
@chromeProcs = SELECT * FROM @procs WHERE Executable like "%chrome%";
```
Everything from the word `SELECT` all the way to the semi-colon `;` at the end is ultimately sent to SQLite.  The rest is dexcode.

---

