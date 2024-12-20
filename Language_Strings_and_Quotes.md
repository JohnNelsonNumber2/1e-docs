# Strings
Strings are simply a sequence of characters which can include letters, numbers, special characters, symbols, etc.

## Double-quotes
Double-quotes `"` always denote the beginning and end of string, even if you're inside of a SQLite statement.
```c
                   Double-quotes
                   ╔═══════════╗
@myString = SELECT "ABCabc-123!" AS StringColumnName; //around string columns


@procs = OperatingSystem.GetProcesses();
SELECT
    *
FROM
    @procs
WHERE
    [Executable] LIKE "%chrome%"; //around strings in WHERE clauses. Not ' single-quotes like you'd expect
                      ╚════════╝
                     Double-quotes
```

## Escape special characters with `\`
If the string has a `\` backslash or a `"` double-quote in it, you must put a `\` backslash in front of it
```c
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
