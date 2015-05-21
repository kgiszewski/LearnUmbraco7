#Core Property Value Converters#
##By Jeavon Leopold##

###What is it?###
It is a package that takes advantage of some of the core's internal plumbing to return useful objects as opposed to simple identifiers.

###Why should I use it?###
An example would be retrieving stored data from the multinode tree picker data type.  Normally that data type will return a comma separated list of node Id's that then need to be parsed into `IPublishedContent` items like so:

```c#
    <ul>
    @{
        var mntpCsvList = Model.Content.GetPropertyValue<string>("mntpPropertyAlias");
        
        foreach(var id in mntpCsvList.Split(','))
        {
            var umbracoContent = Umbraco.TypedContent(id);
            
            <li>@umbracoContent.Name</li>
        }
    }
    </ul>
```

With this package you could rewrite it as such:
```c#
    <ul>
        @foreach(var contentin Model.Content.GetPropertyValue<IEnumerable<IPublishedContent>>("mntpPropertyAlias"))
        {
            <li>@content.Name</li>
        }
    </ul>
```

There are several of these conversions that will happen when this package is installed.  Please consult the package documentation for more details.

###Where do I get it?###

**NuGet:** https://www.nuget.org/packages/Our.Umbraco.CoreValueConverters/

**Our Umbraco:** https://our.umbraco.org/projects/developer-tools/umbraco-core-property-value-converters

[<Back Overview](README.md)

[Next> 02 - uSync](02 - uSync.md)