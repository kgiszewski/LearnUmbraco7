#Search Results#
To perform a query and then show the results, we simply can just create a searcher and call it from a view.  We will first need to define a searcher.  Inside this searcher we need to generate Lucene query commands as you'll see in the example:

```c#
using System;
using System.Collections.Generic;
using Examine;

namespace MyNamespace
{
    public class MySearcher
    {
        public ISearchResults Search(string keywords)
        {
            //determine which searcher we'll use, typically it's the external one
            var searcher = ExamineManager.Instance.SearchProviderCollection["ExternalSearcher"];

            //this changes the behavior of the search result boolean logic
            var searchCriteria = searcher.CreateSearchCriteria(Examine.SearchCriteria.BooleanOperation.Or);

            //create a list that accumulates raw Lucene statements
            var rawQueries = new List<string>();

            if (!string.IsNullOrWhiteSpace(keywords))
            {
                //split keywords by space
                var words = keywords.Split(new[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);

                //for each keyword, let's perform a separate search
                foreach (var word in words)
                {
                    //let me describe in English what's going on
                    //+__IndexType:content -- it must be content
                    //-hideFromSearch:1 -- it must not have a property named hideFromSearch that has a 1
                    //-template:0 -- it must have a template
                    //(nodeName: {0}~{1} content: {0}~{1} alternateName: {0}~{1}) -- it CAN match property nodeName OR content OR alternateName with this word and it's fuzzyness must be in range
                    var contentRawQuery = string.Format("(+__IndexType:content -hideFromSearch:1 -template:0 && (nodeName: {0}~{1} content: {0}~{1} alternateName: {0}~{1}))", word, 0.5);

                    //+__IndexType:media -- must be media
                    //-__NodeTypeAlias:folder -- can't be a folder
                    //(nodeName:{0}~{1}) -- it CAN match the nodeName with fuzzyness
                    var mediaRawQuery = string.Format("(+__IndexType:media -__NodeTypeAlias:folder && (nodeName:{0}~{1}))", word, 0.5);
                    rawQueries.Add(contentRawQuery + " OR " + mediaRawQuery);
                }
            }

            //let's join the content and media queries since we want both to show in this case
            var query = searchCriteria.RawQuery("(" + String.Join(")(", rawQueries) + ")");

            return searcher.Search(query);
        }
    }
}
```

>Examine also has a fluid API to use if you don't want to use raw Lucene syntax.  Learning the Lucene syntax makes for more targeted results but is yet another thing to learn and some prefer the fluid syntax instead. For more on this please consult the official documentation.

##Call the Searcher##
You can call the searcher right from a view or from a controller.  The example below is from a view:

```c#
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@using MyNamespace
@{
    Layout = "base.cshtml";
    var q = HttpContext.Current.Request.QueryString["q"];
    
    var searcher = new MySearcher();
    IEnumerable<SearchResult> results = null;
    
    if(q != null)
    {
        q = q.Trim();
    }
    
    if(string.IsNullOrEmpty(q))
    {
        HttpContext.Current.Response.Redirect("/");
    }

    results = searcher.Search(q).OrderByDescending(x => x.Score);
}

<div>
   <h4>Your search results for "<em>@q</em>":</h4>
   <h5>@results.Count() Result(s)</h5>

    <ol>
        @foreach(var item in results)
        {
            var umbracoItem = Umbraco.TypedContent(item.Id);
            
            <li>
                <a href="@umbracoItem.Url">@umbracoItem.Name (@umbracoItem.Parent.Name)</a>
                <p class="wiki-post-meta">Last Edited: @umbracoItem.UpdateDate</p>
                @{
                    var content = umbracoItem.GetPropertyValue("content");

                    if (content != null)
                    {
                        <p>
                            <small>@Html.Raw(content.ToString())</small>
                        </p>
                    }
                }
            </li>    
        }
    </ol>
</div>
```
>Note that the Examine search result object includes a `Score` property that represents the relevancy score.

[<Back 01 - Built-in Functionality](01 - Built-in Functionality.md)

[Next> 03 - Debugging with Luke](03 - Debugging with Luke.md)