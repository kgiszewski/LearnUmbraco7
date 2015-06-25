#Global.asax#

The `Global.asax` file in Umbraco overrides the typical MVC implementation that usually takes the form of the `Global.asax.cs` code-behind file.

If we peek into the default implementation of that file we see this:

```
<%@ Application Inherits="Umbraco.Web.UmbracoApplication" Language="C#" %>
```

What this means is even if you have a code-behind file present, it won't get used because the `Global.asax` file is pointing to a custom Umbraco class.

One issue that comes up every now and then is the fact that you may need to tie into some of the things normally placed into the code-behind.

To do so you will need to generate a new class that inherits from `UmbracoApplication` and then change the `Inherits` attribute to point to the new class.  Let's run through an example.

First let's create a class that inherits from `UmbracoApplication`, this allows us to preserve core functionality and then gives us an opportunity to chuck some code in.

```
using System;
using Umbraco.Web;

namespace MyNamespace
{
    public class MyApplication : UmbracoApplication
    {
        protected new void Application_Error(object sender, EventArgs e)
        {
            base.Application_Error(sender, e);

            var lastError = Server.GetLastError();

            if (lastError != null)
            {
                //Do something here like send an email
            }
        }
    }
}
```
>Note that you can actually configure Log4Net to send emails if you wanted to avoid having to do this.

Next let's tell Umbraco about our new class by editing the `Global.asax` file as such:

```
<%@ Application Inherits="MyNamespace.MyApplication" Language="C#" %>
```

You aren't limited to just the `Application_Error` method, you can override many more as Chris Randall shows us in this gist: https://gist.github.com/csharpforevermore/7bafb204eb5eb093833a

[<Back 04 - Custom Membership Providers](04 - Custom Membership Providers.md)