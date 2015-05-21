#Overview#

![7377957044_76c6714b63_o.jpg](assets/7377957044_76c6714b63_o.jpg)
>Photo by Doug Robar

AngularJs powers most of the Umbraco 7 backoffice.  If you are new to AngularJs, then you will need to study up on how Angular works [separately](https://angularjs.org/).

There are two other libraries available to you when working with Angular.  They are jQuery and [Underscore.js](http://underscorejs.org/).  You may see usage of both in some of the examples and you should be aware of the mixing of the libraries.

AngularJs is an MVVW framework that appears to use black magic.  This section won't make you a superhero, but it should get you going in the right direction.

The components you should be aware of are:

* `package.manifest` - A file that is read in by the core to register javascript and CSS files that are made available at runtime.
* Controllers - Javascript files that handle things like click events.
* Models - Javascript objects that hold data.
* Directives - New tricks (functionality) we add to HTML.
* Views - Markup that has Angular directives and regular HTML that display your model and trigger actions to be run in your controllers.
* Resources - Fancy way of saying 'get some data'.
* Services - Fancy way of saying do some server stuff via AJAX (most likely).

For this section, we will build a simple property that hides the tab that it appears on.   You can use this bit of functionality to hide the 'Generic Properties' tab on document types that don't represent pages (just data).

[Next> 01 - Manifest](01 - Manifest.md)