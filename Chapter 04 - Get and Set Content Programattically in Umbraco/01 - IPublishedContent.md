# IPublishedContent

This interface is responsible for delivering cached published content to your views.  This interface is `read only` so if you need to manipulate your data please see the section on `IContent`.

Please see the [templates section](/Chapter%2003%20-%20Document%20Types,%20Templates%20and%20Data%20Types/03%20-%20Templates.md) for more information on how to use this class or consult the official Umbraco documentation.
You can access the `IPublishedContent` items elsewhere in your code by simply using the `UmbracoHelper` like so:

```c#
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

[Next> 02 - IContent](02%20-%20IContent.md)
