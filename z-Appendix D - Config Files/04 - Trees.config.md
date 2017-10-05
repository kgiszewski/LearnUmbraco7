# Trees.config

This file is where you would register a custom tree that lives in a section.  The trees can get added to an existing section, or it can get added to your shiny new section.  The following is a brief excerpt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<trees>
  <!--Content-->
  <add initialize="true" sortOrder="0" alias="content" application="content" title="Content" iconClosed="icon-folder" iconOpen="icon-folder"
  	type="Umbraco.Web.Trees.ContentTreeController, umbraco"/>
  <add initialize="false" sortOrder="0" alias="contentRecycleBin" application="content" title="Recycle Bin" iconClosed="icon-folder" iconOpen="icon-folder"
  	type="umbraco.cms.presentation.Trees.ContentRecycleBin, umbraco"/>
 ...
</trees>
```

Same thing applies with trees as does with applications (section), you can manually edit this file or use an [attribute in code](/Chapter%2016%20-%20Custom%20Sections,%20Trees%20and%20Actions/02%20-%20Create%20a%20Tree.md).

[<Back 03 - Applications.config](03%20-%20Applications.config.md)

[Next> 05 - Dashboard.config](05%20-%20Dashboard.config.md)