#IPublishedContent#

This interfaceis responsible for delivering cached published content to your views.  This interface is `read only` so if you need to manipulate your data please see the section on `IContent`.

Please see the [templates section](/Chapter 03 - Document Types, Templates and Data Types/03 - Templates.md) for more information on how to use this class or consult the official Umbraco documentation.
You can access the `IPublishedContent` items elsewhere in your code by simply using the `UmbracoHelper` like so:

```
using Umbraco.Core.Models;
using Umbraco.Web;

namespace SomeNamespace
{
    public class SomeClass
    {
        public void MyMethod()
        {
            var umbHelper = new UmbracoHelper(UmbracoContext.Current);

            IPublishedContent content = umbHelper.TypedContent(1234);
        }
    }
}

```

[<Back Overview](README.md)

[Next> 02 - IContent](02 - IContent.md)