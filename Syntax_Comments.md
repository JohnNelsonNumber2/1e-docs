# Comments
In the dexcode language, comments are used to describe what's going on in the code.  These comments are ignored by the 1E client and serve purely as documentation for programmers.  There are two styles of comments in dexcode:  Single-line or Multiple-line (or block). They use the same characters as javascript or C comments to denote comments.

## Single-Line Comments
Single line comments start with two forward slashes `//`. Everything from the `//` to the end of the line is treated as a comment and ignored by the client.

```
//This is a single-line comment

@agent = Agent.GetSummary(); //this is a single-line comment after the working code

//@device = Device.GetSummary(); //the code on this line is commented out and will not run
```

## Multiple-Line (block) Comments
Multiple-line comments start with forward-slash-star `/*` and end with the reverse star-forward-slash `*/`.  This type of comment is used to provide longer blocks of descriptions or temporarily disable blocks of code without deleting them.

```
/*
    This is a multi-line comment, and a Haiku
      
    Multi-line comment
    Lines of thought, unexecuted
    Silence in the code
*/

@dvc = Device.GetSummary();  // The code that's not in the block will run
```

## Comments within SQLite code
Dexcode is a superset of SQLite (see [Dexcode vs SQLite](./Syntax_Dexcode_vs_SQLite.md) for a description of where SQLite ends and dexcode picks up).

If you trying to put a comment between a `SELECT` and the line ending semi-colon `;` you need to use SQLite comment syntax.

> **NOTE:** Do not use `//` if you're commenting a single line within the SQL code<br>
> SQLite uses `--` to start a single-line comment.<br>

```
@procs = OperatingSystem.GetProcesses();
@chromes =
    SELECT
        /*  
            A c-style SQL block  
            Because it is between the SELECT and the semi-colon
            it needs to follow SQLite syntax
        */
        Executable,
        ProcessId,
        ParentProcessId
    FROM
        @procs prc
    WHERE
        --a single-line SQL comment starts with dash-dash
        prc.Executable LIKE "%chrome%";
```

> **WARNING:** Be careful with single quotes `'` inside of your SQLite comments or you could see errors like this

![](Media/Unterminated%20SQL%20string%20literal.png)

The 1E client takes the code from the instruction and tries to rationalize all of the quotes before it executes the code.  Having a single-quote inside of a SQL comment can mess with that process.  In the above code, if you replace `it is` with `it's` and then run it, you will see the `Unterminated SQL string literal...` error in the image.
