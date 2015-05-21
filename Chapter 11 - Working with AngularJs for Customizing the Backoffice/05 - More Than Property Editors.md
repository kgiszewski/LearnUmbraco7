#More Than Property Editors#
AngularJs is used for more than just property editors.  You can use the manifest file to register javascript for custom trees, sections and context menus items as well.

Make sure to checkout the special section on [Custom Sections, Trees and Actions](/Chapter 16 - Custom Sections, Trees and Actions).

##Custom Dialogs##
One cool feature of working with AnuglarJs and Umbraco is using custom dialogs.  For instance, it would be super awesome if you could click a button and custom dialog appeared.  Then when the user performed an action, the dialog closes and sends an object back to the calling controller.

Well, we're in luck because we can in Umbraco 7.  To do so, we'll turn to our trusty Umbraco Bookshelf project.

The following example assumes you have already registered two controllers.  One for the caller and one for the dialog handler:

```js
//the caller
...
$scope.insertImageMd = function () {
        dialogService.open({
            template: '/App_Plugins/UmbracoBookshelf/dialogs/imageSelector.html',
            show: true,
            callback: function(data) {
               //do something with 'data' after the selection is made
            }
        });
    }
...
```
For the full source, go to: https://github.com/kgiszewski/UmbracoBookshelf/blob/master/src/App_Plugins/UmbracoBookshelf/js/file.controller.js

The snippet above does a few things:
* Opens a dialog
* Specifies the template's location
* Handles the result in a callback

The snippet below is template that will show in the dialog:
```
<div class="umb-dialog" ng-controller="UmbracoBookshelfImageSelectorController">
    <div class="umb-dialog-body" auto-scale="90">

        <div class="bookshelf-image-wrapper">
            <p style="padding: 5px" ng-show="model.images.length == 0">There were no images found in child folders of this content.  Create a folder for your images and add at least one file.</p>
            <div class="" ng-repeat="image in model.images" style="">
                <img ng-src="{{image.FilePath}}" alt="{{image.Alt}}" ng-click="select($index);"/>
            </div>
        </div>
    </div>
</div>
```

Then finally, we need to see the `UmbracoBookshelfImageSelectorController` to see how the dialog sends whatever is selected back to the first controller:

```js
angular.module('umbraco').controller('UmbracoBookshelfImageSelectorController', function ($scope, $routeParams, umbracoBookshelfResource) {

    $scope.model = {};

    $scope.model.currentPath = decodeURIComponent($routeParams.id);

    umbracoBookshelfResource.getImages($scope.model.currentPath).then(function(data) {
        $scope.model.images = data;
    });

    $scope.select = function(index) {
        //this is the magic here
        $scope.submit($scope.model.images[index]);
    }
});
```

What happens is this:
* The function `insertImageMd()` is invoked by the first controller (on it's template https://github.com/kgiszewski/UmbracoBookshelf/blob/master/src/App_Plugins/UmbracoBookshelf/backoffice/UmbracoBookshelfTree/file.html)
* A dialog is opened as a result
* The dialog template is loaded with it's own controller that handles how and what to display
* Clicking an image causes the `$scope.submit()` method to fire which sends back the data to the original callback in the first controller

[<Back 04 - URL Picker Property Editor](04 - URL Picker Property Editor.md)