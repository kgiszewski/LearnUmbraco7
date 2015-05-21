#Umbraco WebAPI#
Umbraco also uses WebAPI and these are more suited for getting information from the backoffice as opposed to surface controllers which are mainly for public information/interactions.

To use an WebApi with Umbraco, you can do the following:

```
using Umbraco.Web.WebApi;

namespace MyNamespace
{
    [AngularJsonOnlyConfiguration]
    public class MyNameController : UmbracoApiController
    {
        public object GetNames()
        {
            var listOfNames = new [] {"Warren", "Bob", "Tom"};

            return listOfNames;
        }
    }
}
```
This will return: 
```
)]}',
["Warren","Bob","Tom"]
```
The extra `)]}` is an AngularJs JSON convention and Angular will automatically strip it off.  If you don't include the `[AngularJsonOnlyConfiguration]` attribute you will get XML back instead, but I found that it is not reliable enough to use that way.

The above controller is public to anyone who has the URL of: `/umbraco/api/myname/getnames`

You can lock down the URL to only resolve to users logged into the backoffice by changing it to:

```
using Umbraco.Web.Editors;

namespace MyNamespace
{
    public class MyNameController : UmbracoAuthorizedJsonController
    {
        public object GetNames()
        {
            var listOfNames = new [] {"Warren", "Bob", "Tom"};

            return listOfNames;
        }
    }
}
```

The difference here is a user must be logged into the backoffice.  If you try to visit the old URL it won't work as the URL for this controller is now: `/umbraco/backoffice/api/myname/getnames`.  If you try to visit that URL with a browser, it'll fail with a 417 code (missing token).  This will only work if you call the URL from AngularJs (which sends a token for you).

You can use the `[PluginController()]` attribute as well (like surface controllers) and your code would look like this:

```
using Umbraco.Web.Editors;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    [PluginController("MyNameThing")]
    public class MyNameController : UmbracoAuthorizedJsonController
    {
        public object GetNames()
        {
            var listOfNames = new [] {"Warren", "Bob", "Tom"};

            return listOfNames;
        }
    }
}
```

Your URL becomes this: `/umbraco/backoffice/mynamething/myname/getnames`

Remember that all WebApi conventions still apply and consult the official Umbraco docs for ways to leverage web services.

[<Back 02 - Surface Controllers](02 - Surface Controllers.md)