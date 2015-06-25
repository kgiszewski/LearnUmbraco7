#Using Sub-folders for Templates#

By default, Umbraco wants to store all of the views for you websites in the `~/views` folder of your web root. Normally this isn't an issue but when you run multiple websites on one install this starts to get very unorganized quickly. Out of the box, Umbraco doesn't support using sub-folders to keep everything nice and tidy. You can however add this functionality yourself by registering a custom view engine like so the following.

First let's create our view engine which is pure .NET MVC and nothing to do with Umbraco (besides the helpers we're using):

```c#
using System.Collections.Generic;
using System.Linq;
using System.Web.Mvc;
using System.IO;
using Umbraco.Core.Logging;
using Umbraco.Core.IO;

namespace MyNamespace
{
    public class MyViewEngine : RazorViewEngine
    {
        public MyViewEngine ()
        {
            var viewsPath = IOHelper.MapPath("~/Views");

            var directories = Directory.GetDirectories(viewsPath);

            var pathList = new List<string>();

            foreach (var directory in directories.Where(x => !x.ToLower().Contains("partials")))
            {
                var folder = Path.GetFileName(directory);

                var path = string.Format("~/Views/{0}/{{0}}.cshtml", folder);

                pathList.Add(path);

                LogHelper.Info<MyViewEngine >("Registering view engine path: " + folder);
            }

            ViewLocationFormats = ViewLocationFormats.Union(pathList).ToArray();
        }
    }
}
```
So what is happening in that custom view engine? Essentially we're reading the `~/views/` directory and finding sub-folders and excluding folders with the word `partials` in it. This will instruct Umbraco to look in the sub-folders for views. So if we have the following structure on the file system:

```
-Views
--Site1
---Site1Base.cshtml
---site1Home.cshtml
--Site2
---Site2Base.cshtml
---Site2Home.cshtml
```

We can then refer to layouts easily like so:

```
//Site1Home.cshtml
@{
   Layout = "Site1Base.cshtml"
}
```
Awesome, now we can have sub-folders in the `~/views` folder that correspond to the individual sites.

Next, let's tell Umbraco about our new view engine:

```c#
using Umbraco.Core;

namespace MyNamespace
{
    public class RegisterViewEngine : ApplicationEventHandler
    {
        protected override void ApplicationStarting(UmbracoApplicationBase umbracoApplication,
            ApplicationContext applicationContext)
        {
            System.Web.Mvc.ViewEngines.Engines.Add(new MyViewEngine());

            base.ApplicationStarting(umbracoApplication, applicationContext);
        }
    }
}
```

One alternative to this approach would be to hijack each document type via custom controller and handle which view is returned. This approach would require a lot of controllers however.