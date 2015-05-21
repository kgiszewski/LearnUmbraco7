#File Service#
The `FileService` will allow for basic file operations related to Umbraco structured items such as:

* Creating Partial Views
* Getting Templates

This class isn't used very often in day to day development.  A sample is provided below:

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

            IEnumerable<ITemplate> allTemplates = services.FileService.GetTemplates();
        }
    }
}
```

[<Back 12 - LogHelper](12 - LogHelper.md)

[Next> 14 - Other Services](14 - Other Services.md)