#MediaService#

`MediaService` will let you fiddle with the media section in a lot of ways. The `MediaService` works with the `IMedia` interface for all CRUD operations.  One common task is to load a media item into Umbraco from another source:

```
using System.IO;
using Umbraco.Core;
using Umbraco.Core.IO;
using Umbraco.Core.Models;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            //grab a reference to services
            var services = ApplicationContext.Current.Services;

            var mediaService = services.MediaService;

            var mediaParentFolder = mediaService.GetById(1234);

            var newMedia = mediaService.CreateMedia("Some name", mediaParentFolder, "Image");

            var tmpFile = System.IO.File.Open(IOHelper.MapPath("~/pathToImage/someImage.png"), FileMode.Open);

            var fileSize = tmpFile.Length;
            var extension = Path.GetExtension(tmpFile.Name).Substring(1);

            newMedia.SetValue("umbracoFile", tmpFile.Name, tmpFile);
            newMedia.SetValue("umbracoBytes", fileSize);
            newMedia.SetValue("umbracoExtension", extension);

            mediaService.Save(newMedia);

            var Id = newMedia.Id; //populated only after save
        }
    }
}
```

##Methods##
There are various methods to be able to grab media such as: `IMedia mediaItem = mediaService.GetById(1234);`.  For a complete list, please consult the official Umbraco docs.

##Reserved Properties##
Umbraco has several reserved properties that are being used by default by several of the `MediaType`'s.  They are as follows:

* `umbracoFile` - Holds the web path to a file in the `~/media` folder
* `umbracoBytes` - Holds the file size in bytes
* `umbracoExtensions` - Holds the extension (without the dot) of the file type

[<Back 07 - Data Type Service](07 - Data Type Service.md)

[Next> 09 - Member Service](09 - Member Service.md)