# Case sensitivity
In general, SCALE is a case-sensitive language.
However, while executing an instruction, some processing [may be](Markdown/Language_SCALE_vs_SQLite.md) handed off to the SQLite engine to do some of the work.
Therefore, it's important to understand when your SCALE code will be case-sensitive and when it will be case-insensitive.

## Table names are case sensitive
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

## Module names are case sensitive
The following SCALE code gets information about the agent from the `GetSummary` method of the `Agent` module.
```
        InitCap
        ╔═════╗
@agent = Agent.GetSummary();
```
Notice the name of the module is `Agent` with a capital A.

If you run that code, everything works properly  
![image](https://github.com/user-attachments/assets/f5e7e33e-178c-4898-888f-52d97aee9a66)

Now consider the following code  
```
         lower
        ╔═════╗
@agent = agent.GetSummary();
```
Now we're referring to the module as `agent` with a lowercase a.

If you run that code, it fails with an error <!-- Not implemented - Unsupported module 'agent' at line 1, column 10 -->  
![image](https://github.com/user-attachments/assets/ef3c1c2e-7a3a-446d-ae13-80d6ff953665)  
This is because the code is trying to reference a module named `agent` but the name of the module is `Agent` and SCALE is case sensitive with regards to module names.

## Method names are case sensitive
The following SCALE code gets information about the disks from the `GetDisks` method of the `Device` module
```
                InitCap
               ╔═══════╗ 
@disks = Device.GetDisks();
```
Notice the name of the method is `GetDisks` with a capital G and D.

If you run that code, everything works properly  
![image](https://github.com/user-attachments/assets/7dcdec94-0e60-41b1-9a71-12af71849e60)

Now consider the following code
```
                 lower
               ╔═══════╗ 
@disks = Device.getdisks();
```
Notice the name of the method is `getdisks` which is all lowercase.

If you run that code, it fails with an error <!-- Not implemented - Unsupported method 'getdisks' (in call to Device.getdisks) at line 1, column 10 -->
![image](https://github.com/user-attachments/assets/7abc61da-2848-4cba-9fa9-12c74f5cc22d)  
This is because the code is trying to reference a method named `getdisks` lowercase but the name of the method is `GetDisks` with a capital G and D and SCALE is case sensitive with regards to method names.

##
