#IOHelper#

The IOHelper only does a few things, but one of the handiest is being able to map a virtual path to a system path:

```
using Umbraco.Core.IO;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            var systemPath = IOHelper.MapPath("~/some-directory");
        }
    }
}
```