#Templates#
Templates are the views that will render your markup, CSS and javascripts.  Umbraco is fully MVC capable but offers a ton of great extensions to get information from the content tree.  Templates can be used by one or more document type.

In order to assign a template to be used with a document type, you first must register the template at `Settings->Templates` in the backoffice.  Most of the time this is done for you automatically when creating a document type.  There is a little check box that is checked by default when creating a document type that creates and associates a template with a document type.

You can edit the template right in the Umbraco backoffice or use Visual Studio.  Either way when a template is registered with Umbraco, it creates a `.cshtml` file in the `~/Views` folder of your web root.

By default all templates will use the `RenderModel` as it's model.  You can alter this by hijacking the route for the document type.  Most of the time your view will just need this at the top:

`@inherits Umbraco.Web.Mvc.UmbracoTemplatePage`

##Accessing Published Content##
There are a lot of methods and extensions that get imported by default into your views.  The `RenderModel` is passed to your view based on the current page's content.  You can access the current model's page or even traverse around the content tree to find data in a relative or absolute fashion.

###Access the Current Page's Content###
The normal way to get content from the current page is via `Model.Content`.

```
string pageName = Model.Content.Name;
int nodeId = Model.Content.Id;
string documentTypeAlias = Model.Content.DocumentTypeAlias;
IPublishedContent parentOfCurrentPage = Model.Content.Parent;
string url = Model.Content.Url;
```

To get data from the data types living on the current page there are two main ways:

```
//this will return a string by default
string value = Model.Content.GetPropertyValue("somePropertyAlias");

//there is an overload that will strongly type the value
var value = Model.Content.GetPropertyValue<int>("somePropertyAlias");

//in some cases you can even do this when you have certain packages installed (https://www.nuget.org/packages/Our.Umbraco.CoreValueConverters/)
var value = Model.Content.GetPropertyValue<IEnumberable<IPublishedContent>>("someMntpPropertyAlias");
```

###Access Data Relative to the Current Page###
Since Umbraco's content is organized in a hierarchy you can traverse the tree like so:

```
//move from current node to level 2 and return the name
Model.Content.Ancestor(2).Name 

//move up the tree and find an ancestor with a particular document type name with a LINQ lambda expression
Model.Content.Ancestors().FirstOrDefault(x => x.DocumentTypeAlias == "foo")

//You can also look downstream
Model.Content.Descendants().FirstOrDefault(x => x.DocumentTypeAlias == "foo")
```

For the complete set of operations you can do to find your data, please visit the official documentation.

###Access Nodes Other Ways###
Built into your views is a helper named UmbracoHelper. It can be used to access a node by Id and other ways:

```
IPublishedContent contentById = Umbraco.TypedContent(1234);
IPublishedContent mediaById = Umbraco.TypedMedia(1234);
IPublishedContent someRootNode = Umbraco.TypedContentAtRoot().FirstOrDefault(x => x.DocumentTypeAlias == "foo");
```

###Sample Template###
Remember that Umbraco is using regular old MVC with a lot of help being given to you.  A sample template might look like this:

```
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@{
    Layout = "base.cshtml";
}

<div>
    @Model.Content.GetPropertyValue("author")
</div>

<div>
    @Model.Content.GetPropertyValue("category")
</div>

<div>
    <h1>List of page links</h1>
    <ul>
        @foreach (var content in Model.Content.Children)
        {
            <li>
                <a href="@content.Url">@content.Name (@content.GetPropertyValue("foo"))</a>
            </li>
        }
    </ul>
</div>

@Html.Partial("~/Views/BizMag/Partials/disqus.cshtml")
```

[<Back 02 - Data Types](02 - Data Types.md)