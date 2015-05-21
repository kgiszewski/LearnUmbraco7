#URL Picker#

At uWestFest 2014 in Las Vegas Nevada, Tom Fulton and Kevin Giszewski did a presentation on building a property editor.  You can find the full details here:  https://github.com/imulus/uWestFest/tree/master/urlpicker

The URL Picker is a simple property editor that does all of what we've covered in the previous chapters:

* [Register the css, js and views](https://github.com/imulus/uWestFest/blob/master/urlpicker/config/package.manifest) with Umbraco.  Note that this manifest registers prevalues that will be available for the editor to configure.
* [The view](https://github.com/imulus/uWestFest/blob/master/urlpicker/app/views/url.picker.html) defines the markup and works in conjunction with the controller.
* [The controller](https://github.com/imulus/uWestFest/blob/master/urlpicker/app/scripts/controllers/url.picker.controller.js) responds to the click events and handles the model.
* The controller handles two special properties of the model that Umbraco recognizes.  `$scope.model` represents the data that will be saved whilst `$scope.model.config` are the prevalues.  Whatever the value of `$scope.model` when the page is saved will be what gets stored in Umbraco.
* This property editor also uses a [Property Value Converter](https://github.com/imulus/uWestFest/blob/master/urlpicker/src/UrlPicker.Umbraco/PropertyConverters/UrlPickerValueConverter.cs) that allows the data to be mapped like so on a template:

```c#
@{
   var urlPicker = Model.Content.GetPropertyValue<UrlPicker.Umbraco.Models.UrlPicker>("myUrlPickerProperty")
   {
        <div>@urlPicker.Url</div>
   }  
}
```

This package can be installed with NuGet at: https://www.nuget.org/packages/UrlPicker/

[<Back 03 - AngularJs Umbraco Services and Resources](03 - AngularJs Umbraco Services and Resources.md)

[Next> 05 - More Than Property Editors](05 - More Than Property Editors.md)