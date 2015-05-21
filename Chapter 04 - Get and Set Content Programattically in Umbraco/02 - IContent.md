#IContent#

The `IContent` interface is the interface you should be using if you want to manipulate the content in the backoffice.  If you wish to just `get` the data, please use the `IPublishedContent` interface family methods.

With `IContent` you can do the following:

* Create content
* Move content
* Sort content
* Rename content
* Delete content
* Rollback content
* Copy content
* Get older versions of content
* Save and publish content

Operations for `IContent` are made available to you via the `ServicesContext` inside Umbraco.  A reference to the services are automatically included in `SurfaceControllers` but you can also get a reference in any class like so:

```
using Umbraco.Core;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            var services = ApplicationContext.Current.Services;
        }
    }
}
```

A typical usage of `IContent` involve the `ContentService`.  In the follow example, a new node is created, a property is set and published.

```
using Umbraco.Core;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            var services = ApplicationContext.Current.Services;

            var parent = services.ContentService.GetById(1234);

            var newContent = services.ContentService.CreateContent("Name of content", parent, "MyDocumentTypeAlias");

            newContent.SetValue("somePropertyAlias", "some value");

            services.ContentService.PublishWithChildrenWithStatus(newContent);

            var Id = newContent.Id;// this is populated after the publish
        }
    }
}
```

>Just remember that `IContent` is considerably slower than `IPublishedContent` due to the database hits.  However it is the interface you should use to create and manipulate content.

For more on the `ServicesContext`, please see [Chapter 05](/Chapter 05 - Helpers, Contexts and Service API's).

[<Back 01 - IPublishedContent](01 - IPublishedContent.md)

[Next> 03 - IMedia](03 - IMedia.md)