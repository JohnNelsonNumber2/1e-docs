# Semi-colon line-endings
Every statement that is not a [comment](./Language_Comments.md) ***must*** end with a `;` semi-colon.
This tells the client where the end of each statement is and allows for the white-space-insensitive functionality below.
```
@device = Device.GetSummary();
//                           ↑ Mandatory semi-colon at the end of every method call...
//                           And at the end of every SELECT statement ↓
@windows = SELECT 1 FROM @device WHERE OperatingSystemType = "Windows";
//                           But no semi-colon at the end of single-line comments →
/*                           And no semi-colon at the end of inline comments →         */

```

# White-space insensitive
SCALE is not white-space sensitive.  An instruction can be written as a single-liner with no carriage-return-line-feeds, or it can be nicely formatted and spread out over multiple lines and it will execute the same.

#### Formatted with line feeds
Below is an example of some code that can be used to prompt a user to click on a URL that will let them reset their password.  

It will create a table of constants that holds the different values that the `Interaction.ShowAnnouncement` method requires and it will popup a dialog with those values.

Notice how there's a [CRLF](./Glossary.md) after each column in the `SELECT` statement that builds the `@const` table.

There's also a `CRLF` between the parameters in the `Interaction.ShowAnnouncement()` method so each parameter is easier to see
```
//constants
@const = SELECT
    "PasswordExpiry.Data" AS DataSlot,
    "Password Expiration" AS AnnouncementName,
    900 AS AnnouncementTimeoutSeconds,
    "Your password will expire in less than {n} days\\n\\nClick on the following link to reset it\\n" AS AnnouncementDescription,
    "https://my.reset.link" AS AnnouncementLink,
    "" AS AnnouncementFooter,
    "Password.Expiration" AS AnnouncementTopic
;
//show password reset dialog
Interaction.ShowAnnouncement(
    Name:@const.AnnouncementName,
    Description:@const.AnnouncementDescription,
    Async:true,
    Topic:@const.AnnouncementName,
    TimeoutSeconds:@const.AnnouncementTimeoutSeconds,
    Link:@const.AnnouncementLink,
    ShowDefaultButtons:false,
    Footer:@const.AnnouncementFooter
);
```

#### One-liner without line feeds
Below is the same code as the formatted example above, but with all of the `CRLF` characters removed
```
/*constants*/@const = SELECT "PasswordExpiry.Data" AS DataSlot, "Password Expiration" AS AnnouncementName, 900 AS AnnouncementTimeoutSeconds, "Your password will expire in less than {n} days\\n\\nClick on the following link to reset it\\n" AS AnnouncementDescription, "https://my.reset.link" AS AnnouncementLink, "" AS AnnouncementFooter, "Password.Expiration" AS AnnouncementTopic;/*show password reset dialog*/Interaction.ShowAnnouncement(Name:@const.AnnouncementName,Description:@const.AnnouncementDescription,Async:true,Topic:@const.AnnouncementName,TimeoutSeconds:@const.AnnouncementTimeoutSeconds,Link:@const.AnnouncementLink,ShowDefaultButtons:false,Footer:@const.AnnouncementFooter);
```
Notice that we've removed all of the white-space and also changed the `//single-line` comments to `/* inline */` comments so it can fit on one line without commenting out valid code.
