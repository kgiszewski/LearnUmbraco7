#FileSystemProviders.config#

This file holds the configuration for file system providers.  In Umbraco it is possible to change where references to files live.  For instance you can change your media to live on Azure.  The following is the normal out of the box config:

```xml
<?xml version="1.0"?>
<FileSystemProviders>
  
  <!-- Media -->
  <Provider alias="media" type="Umbraco.Core.IO.PhysicalFileSystem, Umbraco.Core">
    <Parameters>
      <add key="virtualRoot" value="~/media/" />
    </Parameters>
  </Provider>
   
</FileSystemProviders>

```

To switch media to be stored on Azure, please have a look at [this third party package](/Chapter 10 - First and Third-Party Packages/07 - Azure Blob Storage Provider.md) by Dirk Seefeld.

[<Back 05 - Dashboard.config](05 - Dashboard.config.md)

[Next> 07 - Log4Net.config](07 - Log4Net.config.md)