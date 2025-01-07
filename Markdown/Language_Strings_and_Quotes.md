# Strings
Strings are simply a sequence of characters which can include letters, numbers, special characters, symbols, etc.

## Double-quotes
Double-quotes `"` always denote the beginning and end of string, even if you're inside of a SQLite statement.
```
                                                      /*
                   Double-quotes
                   ╔═══════════╗                      */
@myString = SELECT "ABCabc-123!" AS StringColumnName;


@procs = OperatingSystem.GetProcesses();
SELECT
    *
FROM
    @procs
WHERE
    [Executable] LIKE "%chrome%";   /* double quote in WHERE clause, not single quote like you'd expect
                      ╚════════╝
                     Double-quotes  */                  
```

## Beware of single-quotes in your SQL comments
The 1E client takes the code from the instruction and tries to rationalize all of the quotes before it executes the code or passes it to SQLite. This includes trying to turn single quotes into double quotes since SCALE expects double quotes for strings.  Having a single-quote inside of a SQL comment can mess with that process.  In the following code, the contraction `doesn't` will cause an `Unterminated SQL string literal...` error similar to the image below:
```
@procs = OperatingSystem.GetProcesses();
@chromes =
    SELECT
        Executable,
        ProcessId,
        ParentProcessId
    FROM
        @procs prc
    WHERE
        /* This doesn't return anything but chrome processes */
        prc.Executable LIKE "%chrome%";
```

![image](https://github.com/user-attachments/assets/ac40a455-d31c-4cde-8ff0-e0cb981a5dcb)

Probably best to just avoid single-quotes if you can.

See [Comments](./Language_Comments.md) for more information on SQL comments, or comments in general

## Escape special characters with `\`
If the string has a `\` backslash or a `"` double-quote in it, you must put a `\` backslash in front of it
```
//escape backslash:  \ becomes \\
@myString = SELECT "DOMAIN\\USER" AS Account;

//escape double quotes:  " becomes \"
@myString2 = SELECT "Joe says, \"Hey!\"" AS Greeting;
```

## Escaping pasted code
If you're pasting code from elsewhere into your SCALE string, make sure you escape in the right order:
1. Highlight inside of the string only (not the outer `"` double-quote)<br>![image](https://github.com/user-attachments/assets/942163af-5c92-4dbe-87a1-148d5c3b3882)
2. Type `CTRL+H` to open the `Find and Replace` dialog
3. Make sure `Search Selection` is checked (we don't want to escape the whole instruction, just inside of the string)
4. Replace all `\` with `\\`<br>![image](https://github.com/user-attachments/assets/3a0cf7dd-c0ed-495a-b376-5f96f9652445)
5. Replace all `"` with `\"`<br>![image](https://github.com/user-attachments/assets/f06970ff-746a-4931-9498-a6ca0055a458)
