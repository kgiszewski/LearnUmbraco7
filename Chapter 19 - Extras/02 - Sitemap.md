#Sitemap#

Sitemaps can be done with ease in Umbraco.  By using the alternate template feature in Umbraco we can create a simple site map like so:

* Register a new template in `Settings->Templates` named `sitemap`
* Open the newly created `~/Views/sitemap.cshtml` and add the following:

```
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@{

}<?xml version="1.0" encoding="UTF-8" ?>

<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    @foreach (var page in Model.Content.Descendants().Where(x => x.TemplateId != 0 && !x.GetPropertyValue<bool>("hideFromSearch")))
    {
        <url>
            <loc>@page.UrlAbsolute()</loc>
            <lastmod>@page.UpdateDate</lastmod>
        </url>
    }
</urlset>
```

The feed will show all items with a template associated and exclude any items that have a custom property named `hideFromSearch` with a value of true.

You can visit the sitemap by pointing your browser to: http://mysite.local/sitemap

[<Back 01 - PagedList](01 - PagedList.md)

[Next> 03 - RSS](03 - RSS.md)