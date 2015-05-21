#MemberService#

The `MemberService` is useful if you need to do any member operations.  `MemberServices` works with the `IMember` interface like so:

```
using Umbraco.Core;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            //grab a reference to services
            var services = ApplicationContext.Current.Services;

            var memberService = services.MemberService;

            var member = memberService.GetById(12345);
        }
    }
}
```

There are lots of methods to be able to get members by group or whatever like so: `var members = memberService.GetMembersByGroup("myGroupName");`

Please consult the official Umbraco docs for a full list of methods.

[<Back 08 - Media Service](08 - Media Service.md)

[Next> 10 - Member Type and Member Group Services](10 - Member Type and Member Group Services.md)