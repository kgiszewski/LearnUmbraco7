#RSS#

RSS in Umbraco can be done a number of ways, one way is to take advantage of the alternate template scheme.  To create an RSS feed, do the following:

* Register a new template named `rss` in `Settings->Templates`
* Open the file that is created located at `~/Views/rss.cshtml`
* Add the following code and modify as desired:

```
@using Archetype.Models
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@{
    var currentIssue = Model.Content.GetPropertyValue<IEnumerable<IPublishedContent>>("currentIssue").First();
    
}<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">
    <channel>
        <title>My Magazine</title>
        <link>http://mydomain.local</link>
        <description>My Magazine</description>
        @foreach (var article in currentIssue.Descendants("ArticlePage"))
        {
            var teaseFieldset = article.GetPropertyValue<ArchetypeModel>("modules").FirstOrDefault(x => x.Alias == "richtextModule");
            var tease = "";
            
            if (teaseFieldset != null)
            {
                tease = teaseFieldset.GetValue<string>("text").StripHtml();
            }
            
            <item>
                <title>@article.Name</title>
                @Html.Raw("<link>")@article.UrlAbsolute()@Html.Raw("</link>")
                <description>@tease</description>
            </item>
        }
    </channel>
</rss>
```

This example is using a Archetype fieldset to fill in the `<description>` tag but you can use an Umbraco field.

>Note that this is some special attention with how the `<link>` tag is rendered.  It's sort of a conflict/bug with the C# Razor engine.

Now that you've got a basic RSS feed going, you can check it out by visiting: http://mydomain.local/rss.

>If you use Internet Explorer, it'll format the response in a human readable feed.

[<Back 02 - Sitemap](02 - Sitemap.md)

[Next> 04 - Custom Membership Providers](04 - Custom Membership Providers.md)