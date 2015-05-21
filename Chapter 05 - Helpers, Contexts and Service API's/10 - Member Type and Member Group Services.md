#MemberTypeService and MemberGroupService#

The `MemberTypeService` and `MemberGroupService`'s are where you need to be if you need to get or set member type and group data.

```
using System.Collections.Generic;
using Umbraco.Core;
using Umbraco.Core.Models;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            //grab a reference to services
            var services = ApplicationContext.Current.Services;

            var memberTypeService = services.MemberTypeService;

            IEnumerable<IMemberType> allMembers = memberTypeService.GetAll();

            var memberGroupService = services.MemberGroupService;

            IEnumerable<IMemberGroup> allGroups = memberGroupService.GetAll();
        }
    }
}
```

You can set information programmatically as well but consider just using the GUI for this purpose.

For more information regarding these services, please consult the official docs.

[<Back 09 - Member Service](09 - Member Service.md)

[Next> 11 - IOHelper.md](11 - IOHelper.md.md)