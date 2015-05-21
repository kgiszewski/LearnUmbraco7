#Create a Custom Section#
Creating a custom section involves editing the `~/config/applications.config`.  If we look into that file we can see the core sections defined already plus the last entry for our new section:

```xml
<?xml version="1.0" encoding="utf-8"?>
<applications>
  <add alias="content" name="Content" icon="traycontent" sortOrder="0"/>
  <add alias="media" name="Media" icon="traymedia" sortOrder="1"/>
  <add alias="settings" name="Settings" icon="traysettings" sortOrder="2"/>
  <add alias="developer" name="Developer" icon="traydeveloper" sortOrder="3"/>
  <add alias="users" name="Users" icon="trayusers" sortOrder="4"/>
  <add alias="member" name="Members" icon="traymember" sortOrder="5"/>
  <add alias="forms" name="Forms" icon="icon-umb-contour" sortOrder="6"/>
  <add alias="translation" name="Translation" icon="traytranslation" sortOrder="7"/>
  <add alias="UmbracoBookshelf" name="Umbraco Bookshelf" icon="icon-books" sortOrder="15"/>
</applications>
```

You can select your icon from the built-in icons and adding the alias to the `icon` field.  The `alias` property  will be used later so it is important to choose this wisely.

>In reality we won't actually manually update this file, instead we'll use a built-in attribute in our code to define this.  You can un-register a section by simply editing this file.

You now need to create a class to declare your new section:

```c#
using umbraco.businesslogic;
using umbraco.interfaces;

namespace UmbracoBookshelf.Applications
{
    [Application("UmbracoBookshelf", "Umbraco Bookshelf", "icon-books", 15)]
    public class UmbracoBookshelfApplication : IApplication
    {
    }
}
```

The code above requires the same information in the attribute that the `application.config` requires.

If you run your code now, you'll get no section in the backoffice because you'll need to go into the `Users` section all tick the box for your new section.  Be sure to reload the page after saving the user information.

You'll now notice that your section is visible but it appears to be missing a translation key because it should look like this: `[UmbracoBoookshelf]`.  The brackets indicate you should open up the language file that handles your systems culture and add a key.  Since I'm set up for `en-us` I would edit the file located at `~/umbraco/Config/Lang/en.xml` like so:

```xml
...
<area alias="sections">
    <key alias="UmbracoBookshelf">Umbraco Bookshelf</key>
    <key alias="concierge">Concierge</key>
    <key alias="content">Content</key>
    <key alias="courier">Courier</key>
    <key alias="developer">Developer</key>
    <key alias="installer">Umbraco Configuration Wizard</key>
    <key alias="media">Media</key>
    <key alias="member">Members</key>
    <key alias="newsletters">Newsletters</key>
    <key alias="settings">Settings</key>
    <key alias="statistics">Statistics</key>
    <key alias="translation">Translation</key>
    <key alias="users">Users</key>
    <key alias="help" version="7.0">Help</key>
    <key alias="forms">Forms</key>
    <key alias="analytics">Analytics</key>
  </area>
...
```

Saving that file and reloading your page should fix the brackets problem.

[<Back Overview](README.md)

[Next> 02 - Create a Tree](02 - Create a Tree.md)