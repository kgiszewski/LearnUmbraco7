#SurfaceController#

`SurfaceControllers` are Umbraco's way to provide quick MVC routes for custom behavior.  These are mostly used to handle form requests but can also return text, JSON, XML and views.

Please see the [MVC forms section](/Chapter 07 - Forms/01 - Integrating Umbraco with MVC Forms.md) for using `SurfaceControllers` with forms.

To create a quick web service you can do the following:

```
using System.Collections.Generic;
using System.Web.Mvc;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    public class MyNameSurfaceController : SurfaceController
    {
        public ActionResult GetNames()
        {
            var listOfNames = new List<string>() {"Warren", "Bob", "Tom"};

            return Json(listOfNames, JsonRequestBehavior.AllowGet);
        }
    }
}

```

You can hit that web service at it's default URL: `/umbraco/surface/mynamesurface/getnames`

Which will send back:
```
["Warren","Bob","Tom"]
```

If you would like the url to be a little nicer, you can add an area for routing by adding a `PluginController` attribute like so:

```
using System.Collections.Generic;
using System.Web.Mvc;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    [PluginController("MyNameThing")]
    public class MyNameSurfaceController : SurfaceController
    {
        public ActionResult GetNames()
        {
            var listOfNames = new List<string>() {"Warren", "Bob", "Tom"};

            return Json(listOfNames, JsonRequestBehavior.AllowGet);
        }
    }
}
```

You would then be able to get to the service with this URL: `/umbraco/MyNameThing/mynamesurface/getnames`

Of course the URL is public and if you want to restrict access to members only, you can add another attribute like so:

```
using System.Collections.Generic;
using System.Web.Mvc;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    [MemberAuthorize(AllowAll = true)]
    [PluginController("MyNameThing")]
    public class MyNameSurfaceController : SurfaceController
    {
        public ActionResult GetNames()
        {
            var listOfNames = new List<string>() {"Warren", "Bob", "Tom"};

            return Json(listOfNames, JsonRequestBehavior.AllowGet);
        }
    }
}
```

`SurfaceController`'s are not meant to be secured for the backoffice and are always public.

Surface controllers follow MVC convention for naming and routing.  Please reference official Umbraco and Microsoft documentation.

[<Back 01 - RenderMVC Controllers](01 - RenderMVC Controllers.md)

[Next> 03 - WebAPI Controllers](03 - WebAPI Controllers.md)