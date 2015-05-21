#File System Provider#

So the goal here is to tell Umbraco to stop placing media in `~/media` and to store and serve it from the cloud.

To do so you will need to install the [Azure Blob File System provider](https://our.umbraco.org/projects/backoffice-extensions/azure-blob-storage-provider) by Dirk Seefeld.  I recommend the NuGet version if at all possible.

After installing, you will need to create a new file (or rename the sample) called `~/config/FileSystemProvidersAzure.config`.  You will then need to configure the file with your Azure Blob Storage information:

```
<?xml version="1.0"?>
<FileSystemProviders>
  <Provider alias="media" type="idseefeld.de.UmbracoAzure.AzureBlobFileSystem, idseefeld.de.UmbracoAzure">
    <Parameters>
      <add key="containerName" value="media" />
      <add key="rootUrl" value="" />
      <add key="connectionString" value="DefaultEndpointsProtocol=https;AccountName=;AccountKey="/>
      <add key="mimetypes" value="" />
      <add key="cacheControl" value="*|public, max-age=31536000;js|public, max-age=86400" />
    </Parameters>
  </Provider>
</FileSystemProviders>

```

Next, go to your Azure Blob storage account and create a new `container` called `media`.  

>You can name these whatever you want as long as you remember to change the names as we continue.

Finally, since I don't want my local dev to store images in Azure but I DO want them to be stored in Azure when for production/preprod, I need to setup a web transform.  Add this bit to your `Web.Azure-Prod.config` file:

```
<configuration>
  <umbracoConfiguration>
    <FileSystemProviders configSource="config\FileSystemProvidersAzure.config" xdt:Transform="SetAttributes(configSource)"/>
  </umbracoConfiguration>
</configuration>
```

>Make sure you have set your Azure app setting to use the `Web.Azure-Prod.config` transform (previous section).

It is important to know that using this file system provider will break Image Processor's `GetCropUrl()` functionality within Umbraco.  The next section covers how to fix it.

[<Back 02 - Azure Configuration](02 - Azure Configuration.md)

[Next> 04 - Fixing Image Processor with Umbraco](04 - Fixing Image Processor with Umbraco.md)