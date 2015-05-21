#Fixing Image Processor with Umbraco#
So using the Azure File System package unwittingly breaks Image Processor's ability to make crops.  This is because Image Processor relies on the fact that images are usually hosted in the same web app.  Fortunately James South (and others) have a workaround.  The workaround grabs the Azure hosted images (that live at a different URL now) and proxy's them through Umbraco (thus restoring the ability to crop them).

There is a fun discussion here if you'd like to read further: https://our.umbraco.org/projects/backoffice-extensions/azure-blob-storage-provider/your-remarks,-ideas-etc/64307-Image-CropperImage-Processor-Crops

##The Fix
First you will need to update your `ImageProcessor.Web` (https://www.nuget.org/packages/ImageProcessor.Web/) to the latest version.  At the moment, NuGet is your safest (only?) option.

Next install `ImageProcessor.Web.Config` (https://www.nuget.org/packages/ImageProcessor.Web.Config/)

Go into `~/config/imageprocessor/security.config` and add your storage account to the whitelist: `<add url="http://[accountname].blob.core.windows.net/"/>`.

Next you will need to come up with a way to alter your URL's when serving Azure items.  Perhaps a simple `replace()` or something like the following extension:

```
public static string ToAzureBlobUrl(this string input)
{
    var domain = HttpContext.Current.Request.Url.Host;

    if (domain.EndsWith(".local"))
        return input;

    return input.Replace("://", string.Format("://{0}/remote.axd/", domain));
}
```

The idea is that you are inserting `remote.axd` into the url which tells Image Processor to get the image by proxy.  Thanks to both James and Dirk for helping out here.

Usage would be: `someImageIPublishedContent.GetCropUrl().ToAzureBlogUrl()`.

[<Back 03 - File System Provider](03 - File System Provider.md)

[Next> 05 - Wiring Up Image Processor Cache](05 - Wiring Up Image Processor Cache.md)