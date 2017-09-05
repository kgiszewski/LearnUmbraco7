# Applications.config

This file is where Umbraco stores section configurations also known as `applications`.

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
  <add alias="MyApplication" name="My Section" icon="icon-myicon" sortOrder="15"/>
</applications>
```

While you can edit this section manually to add a section, most developers use the attribute for an application class instead.  Please see the [custom section](/Chapter%2016%20-%20Custom%20Sections,%20Trees%20and%20Actions/01%20-%20Create%20a%20Section.md) area.

[<Back 02 - UrlRewriting.config](02%20-%20UrlRewriting.config.md)

[Next> 04 - Trees.config](04%20-%20Trees.config.md)
