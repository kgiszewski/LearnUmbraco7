#ContentTypeService#

So where `ContentService` is for manipulating `IContent`, `ContentTypeService` is used to manipulate backoffice document types via the `IContentType` interface.

This services is used for both document types and media types.  This service is useful for finding a document type by alias and several other uses.  Please consult the official documents for a complete set of methods.

```
using System.Linq;
using Umbraco.Core;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            //grab a reference to services
            var services = ApplicationContext.Current.Services;

            var contentTypeService = services.ContentTypeService;

            var homePageContentType = contentTypeService.GetAllContentTypes().FirstOrDefault(x => x.Alias == "HomePage");
        }
    }
}
```

[<Back 05 - Content Service](05 - Content Service.md)

[Next> 07 - Data Type Service](07 - Data Type Service.md)