#ApplicationContext#

The representation of the application layer in Umbraco.  Typically this class is going to be where you'll get references to the backoffice Service API (`ContentService`, `ContentTypeService`, etc) and the `DatabaseContext`.  Additionally this class handles some app-wide caching.

```
using Umbraco.Core;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            var dBcontext = ApplicationContext.Current.DatabaseContext;

            var services = ApplicationContext.Current.Services;
        }
    }
}
```

[<Back Overview](README.md)

[Next> 02 - UmbracoContext](02 - UmbracoContext.md)