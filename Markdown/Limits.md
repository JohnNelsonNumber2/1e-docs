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

## Length of all tags combined
510 characters (scopable/coverage tags)  
100,000+ characters (non-scopable/free-form tags)

If you look at all tags, before any have been stored, with the command `Tagging.GetAll();` with the parameter `Scopable:True` you will see something like this

![image](https://github.com/user-attachments/assets/f20de233-5ecc-43cd-8e54-860d35009eb2)

So there are already two delimiter-pipes being stored which counts against the 510.  

Now let's call `Tagging.Set(...)` a couple of times to set 2 scopable tags.
```
Tagging.Set(Name:"tag1", Value:"value1", Scopable:True);
Tagging.Set(Name:"tag2", Value:"value2", Scopable:True);
Tagging.GetAll();
```
When you run that and look at it again, you will then see this
  
![image](https://github.com/user-attachments/assets/f30fcab4-171a-4bd8-82ce-27d5937a20b7)

So if we think about a formula for remaining tag space theres (510 total - 2 pipes) - (SUM(tag name length + tag value length + 1 for equal sign) + (Number of tags - 1 for vertical pipes))

Non-scopable tags do not have this limitation.  You can have as many non-scopable tags as you want.  
This has been tested up to 10,000 tags and 100,000 characters but this is not a hard limit.

## Longest tag name
32 characters

When setting a tag, whether it's scopable or non-scopable, the name must be 32 characters or less

## Longest tag value
506 characters (scopable - depending on other tags and the length of the tag name)  
Unlimited (non-scopable/free-form tags, this has been tested to 100,000 characters)

See [Length of all tags combined](#length-of-all-tags-combined) to see how the tags are stored

So, if this is the first tag being saved, and the tag name is a single character like "A", then the maximum size of the tag value would be

2pipes + Length of tag name + equal sign + Length of tag value = 510  
2 + 1 + 1 + Length of tag value = 510  
4 + Length of tag value = 510  
**Max length of tag value = 506**

This obviously shrinks when the name of the tag is longer, and if there are already tags stored.
