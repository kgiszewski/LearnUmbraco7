#Wiring Up Image Processor Cache#

Image Processor stores cached crops in the `~/app_data/cache` folder by default, but you can send this into the storage as well.  The advantage is that when you swap production and preprod, then the crops are already generated and don't have to redo the work.

To set this up, you will need to make sure you create a container named `cache` (or whatever you want) in Azure storage.

Next sign up for a CDN account on Azure.  You will have to wait at least an hour after signing up before it becomes live.

Next setup your `~/config/imageprocessor/cache.config` like so:

```
<?xml version="1.0" encoding="utf-8"?>
<caching currentCache="AzureBlobCache">
  <caches>
    <cache name="AzureBlobCache" type="ImageProcessor.Web.Plugins.AzureBlobCache.AzureBlobCache, ImageProcessor.Web.Plugins.AzureBlobCache" maxDays="365">
      <settings>
        <setting key="CachedStorageAccount" value="DefaultEndpointsProtocol=https;AccountName=;AccountKey=" />
        <setting key="CachedBlobContainer" value="cache" />
        <setting key="CachedCDNRoot" value="[CDN URL]" />
        <setting key="SourceStorageAccount" value="DefaultEndpointsProtocol=https;AccountName=;AccountKey=" />
        <setting key="SourceBlobContainer" value="media" />
      </settings>
    </cache>
  </caches>
</caching>
```

Just make sure to mind which container names you used if you changed them.  Also be sure to add in the CDN  URL.

##Different for Local and Dev
If you want to keep the Azure stuff only on production/preprod, you'll need another transform.  You will also want to have a separate cache config file.

So `cache.config` would be:

```
<?xml version="1.0" encoding="utf-8"?>
<caching currentCache="DiskCache">
  <caches>
    <cache name="DiskCache" type="ImageProcessor.Web.Caching.DiskCache, ImageProcessor.Web" maxDays="365">
      <settings>
        <setting key="VirtualCachePath" value="~/app_data/cache" />
      </settings>
    </cache>
  </caches>
</caching>
```

And then create `cacheAzure.config` with the settings from the previous section.

Finally do a transform inside `Web.Azure-Prod.config`:

```
<configuration>
  <imageProcessor>
    <caching configSource="config\imageprocessor\cacheAzure.config" xdt:Transform="SetAttributes(configSource)"/>
  </imageProcessor>
</configuration>
```

[<Back 04 - Fixing Image Processor with Umbraco](04 - Fixing Image Processor with Umbraco.md)