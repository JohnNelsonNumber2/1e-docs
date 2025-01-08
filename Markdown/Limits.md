# Limits
This describes operating limits/maximums in the language, the [client](./Glossary.md#Client), [TIMS](./Glossary.md#tims) or the [platform](./Glossary.md#platform)

## Automation size
32KB*

*An [automation](./Glossary.md#automation) (PreCondition, Check or Fix) doesn't really have a size limit because it gets sent to clients using the [background channel](./Glossary.md#background-channel) but because TIMS has a limitation of 32K in the code pane, the realistic limit for an automation fragment is 32K unless you're going to author it outside of TIMS

## Instruction size
4KB compressed
 
An [instruction](./Glossary.md#instruction) (question or action) must be at most 4K when compressed.

Instructions are sent to the client via the always-on [switch](./Glossary.md#switch) component which is optimized for speed.

To see how large the instruction is, check the bottom right corner of TIMS. It should show the uncompressed and compressed sizes.
The compressed size is what matters
  
![image](https://github.com/user-attachments/assets/595dc5e6-1fb7-4eeb-bf7f-f2dd99575718)

> [!NOTE]
> When an instruction is invoked from the platform, it's not just the compressed instruction XML that's sent.  It's the parameter names and their values too.
> This means that you may have an instruction that is under the 4K limit, but the amount of data that's passed in the parameters puts it over the 4K limit.

## Maximum result set
100,000 rows
  
When running a SQLite expression, or when running a method which returns a result set, the 1E Client is instructed to stop processing rows after it receives 100,000 rows by default.
This can be changed with a setting the 1E Client config file.  See [help.1e.com](https://help.1e.com) for more information on client settings.

## Maximum file size when reading its content
10MB
  
The method `FileSystem.GetFile()` will get the entire contents of a text file.

By default, the limit for retrieving file content with this method is 10MB of data.  This can be changed with a setting the 1E Client config file.  See [help.1e.com](https://help.1e.com) for more information on client settings

## Maximum file rows when reading line-by-line
50,000 lines
  
The method `FileSystem.GetFileByLine()` will get the contents of a text file line-by-line using CRLF or LF as the delimiter.

This has a hard-coded limit of 50,000 lines and cannot be changed.
