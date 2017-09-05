# Interceptors

Every now and again some things in Umbraco aren't neatly overridden. Take for example the search box at the top left of the backoffice. The normal behavior for this is to use the internal Examine search and give results. However when you are not in the `content` section you may want that box to return custom search results.

## Intercept a View Request

AngularJs provides a way for us to examine `$http` requests to the server which allows us to listen for a request, modify it and return the results back to the browser. The example below allows us to switch out the entire left panel normally found at `views/directives/umb-navigation.html` with a custom view located at `/App_Plugins/UmbracoBookshelf/Views/navigation.html`.

```js
angular.module('umbraco').service('myInterceptor', function() {
    var service = this;

    service.request = function(request) {
        if (request.url == "views/directives/umb-navigation.html") {
            request.url = "/App_Plugins/UmbracoBookshelf/Views/navigation.html";
        }

        return request;
    };
});

angular.module('umbraco').config(function ($httpProvider) {
    $httpProvider.interceptors.push('myInterceptor');
});
```

While this is probably considered a hacky nuclear option, it *does* provide a way to get around limitations normally imposed on us. For the example above, this allows us to copy and paste the original view into a new view and we can start modifying it to suit our needs.

The best part? We didn't modify the core at all! Just remove your code and the normal functionality returns.

[<Back 05 - More Than Property Editors](05%20-%20More%20Than%20Property%20Editors.md)
