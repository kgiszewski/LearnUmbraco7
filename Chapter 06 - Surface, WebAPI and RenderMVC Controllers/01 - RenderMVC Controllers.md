#RenderMvcController#

This controller is the default controller whenever any content in Umbraco is requested.  It will automatically get the correct view based on what is selected in the content tree.  By default the model will be `RenderModel` which is the the representation of your document types' data types.

You can override this controller by *hijacking* this behavior by document type or by template.

>When considering to hijack a route (by doctype or template), understand that you can achieve custom behavior by simply adding some logic to a view instead.  While this will not keep the MVC God's happy, it will result in much less code if you have a lot of document types.

##Hijack by Document Type##

So the idea is that all requests will be handled by `RenderMvcController` except a document type named `HomePage`.

```
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
###Return a Custom Model###
So just like before except we send back a custom model.  Our model will have all of the information from the original `RenderModel`, except now we've added a new property.

```
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

Then you're view will need to be set up like this:
```
@inherits Umbraco.Web.Mvc.UmbracoViewPage<MyNamespace.CustomRenderModel>
@{
    Layout = null;
}
<div>@Model.Content.Name</div>
<div>@Model.MyNewProperty</div>

```

##Hijack by Template##
The next example shows how to hijack by template name.  Essentially you just name the method after the template alias:

```
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

##Global Replacement of the RenderMvcController##
You can even replace the default controller if you want.  If you need to do that, please consult the official Umbraco documents.

[<Back Overview](README.md)

[Next> 02 - Surface Controllers](02 - Surface Controllers.md)