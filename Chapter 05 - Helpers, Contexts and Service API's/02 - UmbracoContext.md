# UmbracoContext

The `UmbracoContext` is a reference to Umbraco in code.  This class isn't used directly all that much, but the following class illustrates some possible uses:

```c#
using Umbraco.Core.Models;
using Umbraco.Web;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
             
            var httpContext = UmbracoContext.Current.HttpContext;
            var applicationContext = UmbracoContext.Current.Application;
            IPublishedContent content = UmbracoContext.Current.ContentCache.GetById(1234);
            var isInPreviewMode = UmbracoContext.Current.InPreviewMode;

            //the umbraco helper takes the UmbracoContext in the constructor
            var umbHelper = new UmbracoHelper(UmbracoContext.Current);
        }
    }
}
```

[<Back 01 - ApplicationContext](01%20-%20ApplicationContext.md)

[Next> 03 - DatabaseContext](03%20-%20DatabaseContext.md)
