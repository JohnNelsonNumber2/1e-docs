# Case sensitivity
In general, SCALE is a case-sensitive language.
However, while executing an instruction, some processing [may be](Markdown/Language_SCALE_vs_SQLite.md) handed off to the SQLite engine to do some of the work.
Therefore, it's important to understand when your SCALE code will be case-sensitive and when it will be case-insensitive.

## Table names are case sensitive
The following SCALE code gets information about the device from the `GetSummary` method of the `Device` module.
```
                              /*
 LOWER
╔═════╗                       */
@device = Device.GetSummary();
SELECT * FROM @device;        /*
              ╚══════╝
               LOWER          */
```
Notice that we're writing to a table called `@device` which is all lowercase  
and we're selecting from `@device` which is also all lowercase.  
  
If you run that code, everything works properly  
![image](https://github.com/user-attachments/assets/84261418-66d2-46e1-9f15-ba088e3a3567)

  
Now consider the following code
```
                              /*
 UPPER
╔═════╗                       */
@DEVICE = Device.GetSummary();
SELECT * FROM @device;        /*
              ╚══════╝
               lower          */
```
If you run that code, it fails with an error <!-- Unknown variable @device at line 2, column 1 -->  
![image](https://github.com/user-attachments/assets/9385607d-b60c-4b94-a651-0d8730f7296c)  
This is because the code writes to a table called `@DEVICE` which is all uppercase  
but selects from `@device` which is all lowercase.  Table names are case-sensitive so it fails.

## Module names are case sensitive
The following SCALE code gets information about the agent from the `GetSummary` method of the `Agent` module.
```
                            /*
        InitCap
        ╔═════╗             */
@agent = Agent.GetSummary();
```
Notice the name of the module is `Agent` with a capital A.

If you run that code, everything works properly  
![image](https://github.com/user-attachments/assets/f5e7e33e-178c-4898-888f-52d97aee9a66)

Now consider the following code  
```
                            /*
         lower
        ╔═════╗             */
@agent = agent.GetSummary();
```
Now we're referring to the module as `agent` with a lowercase a.

If you run that code, it fails with an error <!-- Not implemented - Unsupported module 'agent' at line 1, column 10 -->  
![image](https://github.com/user-attachments/assets/ef3c1c2e-7a3a-446d-ae13-80d6ff953665)  
This is because the code is trying to reference a module named `agent` but the name of the module is `Agent` and SCALE is case sensitive with regards to module names.

## Method names are case sensitive
The following SCALE code gets information about the disks from the `GetDisks` method of the `Device` module
```
                           /*
                InitCap
               ╔════════╗  */
@disks = Device.GetDisks();
```
Notice the name of the method is `GetDisks` with a capital G and D.

If you run that code, everything works properly  
![image](https://github.com/user-attachments/assets/7dcdec94-0e60-41b1-9a71-12af71849e60)

Now consider the following code
```
                           /*
                 lower
               ╔════════╗  */
@disks = Device.getdisks();
```
Notice the name of the method is `getdisks` which is all lowercase.

If you run that code, it fails with an error <!-- Not implemented - Unsupported method 'getdisks' (in call to Device.getdisks) at line 1, column 10 -->  
![image](https://github.com/user-attachments/assets/7abc61da-2848-4cba-9fa9-12c74f5cc22d)  
This is because the code is trying to reference a method named `getdisks` lowercase but the name of the method is `GetDisks` with a capital G and D and SCALE is case sensitive with regards to method names.

## Method parameter names are case sensitive
The following SCALE code pops up a dialog to the user asking a yes or no question
```
                                                                                                              /*
                                                  InitCap
                                               ╔═══════════╗                                                  */
@answer = Interaction.ShowQuestion(Async:False, Description:"Do you want tacos?", Title:"Important Question");
```
Notice the `Description` parameter has a capital D.

If you run that code, everything works properly.  
You can see there's a popup  
  
![image](https://github.com/user-attachments/assets/7b033ba1-54cf-4c58-9620-2485c5f3bb22)  
and when the user enters `Yes` or `No`, it will return a result set indicating if they've clicked yes, no or if it timed out  
  
![image](https://github.com/user-attachments/assets/98a890b2-4576-4834-bf32-637009f28481)

Now consider the following code
```
                                                                                                              /*
                                                   lower
                                               ╔═══════════╗                                                  */
@answer = Interaction.ShowQuestion(Async:False, description:"Do you want tacos?", Title:"Important Question");
```
Notice the `description` parameter is all lowercase

If you run that code, it fails with an error <!-- Error - Missing parameter 'Description' (in call to Interaction.ShowQuestion) at line 1, column 11 -->  
![image](https://github.com/user-attachments/assets/bec6c5c3-3990-4635-b379-429012dc0ed9)  
This is because the code is calling `ShowQuestion` which expects a parameter named `Description` to be passed in, but we're passing in `description` instead and SCALE is case sensitive with regards to parameter names.

## Method parameter values - IT DEPENDS
Method parameter values are sometimes case sensitive, and sometimes not.  It really depends on the function and the datatype it's expecting.
We can't go into every possible way that parameter values are or aren't case sensitive because there are so many permutations of methods and parameters and operating systems that we just can't provide an exhaustive list.
But there are a few things we can call out.

### Boolean parameter values are case insensitive
Parameters that are expecting a boolean `True` or `False` are not case sensitive.
Consider the `SplitLines` method in the `Utilities` module. The `RemoveEmptyEntries` parameter expects either `True` or `False`.
```
                                                                                /*
                                                                    Insensitive
                                                                      ╔═════╗   */
Utilities.SplitLines(Text:"a,b,c,d", Delimiter:",", RemoveEmptyEntries:True);
Utilities.SplitLines(Text:"a,b,c,d", Delimiter:",", RemoveEmptyEntries:true);
Utilities.SplitLines(Text:"a,b,c,d", Delimiter:",", RemoveEmptyEntries:TRUE);
Utilities.SplitLines(Text:"a,b,c,d", Delimiter:",", RemoveEmptyEntries:False);
Utilities.SplitLines(Text:"a,b,c,d", Delimiter:",", RemoveEmptyEntries:false);
Utilities.SplitLines(Text:"a,b,c,d", Delimiter:",", RemoveEmptyEntries:FALSE);
```
Capitalization isn't important for that value. All of the above permutations of true/false are completely acceptable.

### Parameter values that reference tables are case sensitive
If we consider the `SplitLines` example above, you'll see that we're passing the string `"a,b,c,d"` to be split.
That value isn't case sensitive because it will just split any string on a `,` comma delimiter.

However, if we put that string in a table and reference the table, the table name and column name is case sensitive  
```
                                                                                  /*
       Put string in table
╔══════════════════════════════════╗                                              */
@string = SELECT "a,b,c,d" AS Value;
Utilities.SplitLines(Text:@string.Value, Delimiter:",", RemoveEmptyEntries:True); /*
                         ╚═════════════╝
                       Sensitive Reference                                        */
```

### String parameter values may be case sensitive
It will depend on the method and the OS, but you could consider methods which reference files on Linux, Unix or another *nix-based OS like MacOS as being potentially case sensitive.  
Consider the following code
```
FileSystem.GetFileByLine(FilePath:"c:\\Windows\\CCM\\Logs\\ccmexec.log"); //path is insensitive on Windows
```
That code will read the contents of the ccmexec.log.
On Windows, that FilePath value could be all caps or all lowercase and Windows will still be able to resolve it.  

But if you did something similar on a Linux/Unix-based OS where the filesystem is natively case-sensitive, a path with the wrong capitalization might return an empty table because the path capitalized doesn't exist, but the lowercase path does
```
                                                        /*
                                      VALID PATH
                                  ╔═════════════════╗   */
FileSystem.GetFileByLine(FilePath:"/var/log/kern.log");
FileSystem.GetFileByLine(FilePath:"/VAR/LOG/KERN.LOG"); /*
                                  ╚═════════════════╝
                                     INVALID PATH       */
```
The above code might not throw an error, but if the path is invalid due to capitalization, then it would return an empty table.
