# RenderMvcController

This controller is the default controller whenever any content in Umbraco is requested.  It will automatically use the view defined by the content node's template property.  By default the model will be `RenderModel` which is the representation of the content node.

You can override this controller by *hijacking* this behavior by document type or by template.

>When considering to hijack a route (by doctype or template), understand that you can achieve custom behavior by simply adding some logic to a view instead.  While this will not keep the MVC God's happy, it will result in much less code if you have a lot of document types.

## Hijack by Document Type

To hijack a route by document type, create a class that extends `RenderMvcController` with a name that starts with the document type name and ends with `Controller`.  This example will hijack requests for content of the document type named `HomePage`:

```c#
using System.Web.Mvc;
using Umbraco.Core.Logging;
using Umbraco.Web.Models;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    public class HomePageController : RenderMvcController
    {
        public override ActionResult Index(RenderModel model)
        {
            LogHelper.Info<HomePageController>("Hello from a hijacked controller.");

            //this line means continue and perform the normal rendering
            return base.Index(model);
        }
    }
}
```
### Return a Custom Model
You can use hijacking to send back a custom model.  Our model will have all of the information from the original `RenderModel`, except now we've added a new property.

```c#
using System.Web.Mvc;
using Umbraco.Core.Logging;
using Umbraco.Web.Models;
using Umbraco.Web.Mvc;
using Umbraco.Web;

namespace MyNamespace
{
    public class HomePageController : RenderMvcController
    {
        public override ActionResult Index(RenderModel model)
        {
            LogHelper.Info<HomePageController>("Hello, we have a custom model.");

            //this line means continue and perform the normal rendering
            return base.Index(new CustomRenderModel());
        }
    }

    public class CustomRenderModel : RenderModel
    {
        public string MyNewProperty;

        public CustomRenderModel()
            : base(UmbracoContext.Current.PublishedContentRequest.PublishedContent)
        {
            MyNewProperty = "Umbraco rocks!";       
        }
    }
}
```

Then your view will need to be set up like this:
```c#
@inherits Umbraco.Web.Mvc.UmbracoViewPage<MyNamespace.CustomRenderModel>
@{
    Layout = null;
}
<div>@Model.Content.Name</div>
<div>@Model.MyNewProperty</div>

```

>This technique can be used to create strongly typed view models. There are a few packages, such as [UmbracoMapper](https://our.umbraco.org/projects/developer-tools/umbraco-mapper/) and [Ditto](https://our.umbraco.org/projects/developer-tools/ditto/), that can be used for creating strongly typed view models.

## Hijack by Template
The next example shows how to hijack by template name.  Essentially you just name the method after the template alias:

```c#
using System.Web.Mvc;
using Umbraco.Core.Logging;
using Umbraco.Web.Models;
using Umbraco.Web.Mvc;
using Umbraco.Web;

namespace MyNamespace
{
    public class HomePageController : RenderMvcController
    {
        public override ActionResult Index(RenderModel model)
        {
            LogHelper.Info<HomePageController>("Hello, we have a custom model.");

            //this line means continue and perform the normal rendering
            return base.Index(new CustomRenderModel());
        }

        public ActionResult HomePage(RenderModel model)
        {
            LogHelper.Info<HomePageController>("Hello, we have a custom model AND we're hijacking by template.");

            //this line means continue and perform the normal rendering
            return base.Index(new CustomRenderModel());
        }
    }

    public class CustomRenderModel : RenderModel
    {
        public string MyNewProperty;

        public CustomRenderModel()
            : base(UmbracoContext.Current.PublishedContentRequest.PublishedContent)
        {
            MyNewProperty = "Umbraco rocks!";       
        }
    }
}
```

## Global Replacement of the RenderMvcController
You can even replace the default controller if you want.  If you need to do that, please consult the official Umbraco documents.

[<Back Overview](README.md)

[Next> 02 - Surface Controllers](02 - Surface Controllers.md)
