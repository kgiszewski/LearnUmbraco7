#UmbracoHelper#

The `UmbracoHelper` is the class that enables easy access to `IPublishedContent` content.  Here are some of the things you can do:

* Get `IPublishedContent` by Id
* Get media by Id
* Get a member by Id
* Get root content

For a complete list, see the official Umbraco documentation.

You automatically get a reference to the `UmbracoHelper` in views and can be used like this:

```
@Umbraco.TypedContent(1234).Name
@Umbraco.TypedMedia(1235).GetCropUrl();
```

You get a reference for free in `SurfaceController`'s as well:

```
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    public class MyClass : SurfaceController
    {
        public void MyMethod()
        {
            Umbraco.TypedContent(1234);
        }
    }
}
```

You can also get a reference in any class:

```
using Umbraco.Web;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            var umbHelper = new UmbracoHelper(UmbracoContext.Current);

            var content = umbHelper.TypedContent(1234);
        }
    }
}
```

[<Back 03 - DatabaseContext](03 - DatabaseContext.md)

[Next> 05 - Content Service](05 - Content Service.md)