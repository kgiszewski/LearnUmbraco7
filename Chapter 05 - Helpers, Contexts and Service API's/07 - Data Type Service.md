#DataTypeService#
Data type service will help you programmatically grab information about your site's configured property editors.

A common use is to get a data type and it's prevalues:

```
using System.Linq;
using Umbraco.Core;
using Umbraco.Core.Logging;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            //grab a reference to services
            var services = ApplicationContext.Current.Services;

            var dataTypeService = services.DataTypeService;

            var allDropdowns =
                dataTypeService.GetAllDataTypeDefinitions()
                    .Where(x => x.PropertyEditorAlias == "Umbraco.DropDown");

            var myDropdown = allDropdowns.First(x => x.Name == "My Super Special Dropdown");

            var prevalues = dataTypeService.GetPreValuesCollectionByDataTypeId(myDropdown.Id);

            foreach (var prevalue in prevalues.PreValuesAsDictionary)
            {
                LogHelper.Info<MyClass>(prevalue.Value.Id.ToString());
                LogHelper.Info<MyClass>(prevalue.Value.Value);
            }
        }
    }
}
```

[<Back 06 - Content Type Service](06 - Content Type Service.md)

[Next> 08 - Media Service](08 - Media Service.md)