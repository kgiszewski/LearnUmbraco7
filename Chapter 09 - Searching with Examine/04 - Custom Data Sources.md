#Custom Data Sources#

Examine is a really fast and powerful provider of the Lucene search engine. What makes it even more amazing is the ease of creating an index from any data source. For instance if you wanted to return search results based on database records or file system files, you can.

The idea is that we will create a completely new index in Umbraco that returns search results from any searcher that uses the new index. This example is taken from how the Bookshelf project is doing it so without further delay let's list out the tasks that need to be done:

1. Implement `ISimpleDataService`
2. Edit the config files to tell Umbraco/Examine about our new stuff
3. Search it

##Implement ISimpleDataService##
Implementing `ISimpleDataService` involves creating a class like so:
```c#
using System.Collections.Generic;
using Examine;
using Examine.LuceneEngine;
using Umbraco.Core.Logging;

namespace MyNamespace
{
    public class BookshelfExamineDataService : ISimpleDataService
    {
        public IEnumerable<SimpleDataSet> GetAllData(string indexType)
        {
            var data = new List<SimpleDataSet>();

            LogHelper.Info<BookshelfExamineDataService>("Building index...");

            //this only adds one item to the index, but create a loop and go to town
            data.Add(new SimpleDataSet()
            {
                NodeDefinition = new IndexedNode()
                {
                    NodeId = 1, //a unique identifier
                    Type = "Bookshelf" //this should match the `type` in the ~/config/ExamineSettings.config
                },
                RowData = new Dictionary<string, string>()
                {
                    //this is a simple dictionary based on the ~/config/ExamineIndex.config file
                    //you can customize this however you want but the config should match this dictionary
                    {"book", "A Book"},
                    {"path", "/path/to/book"},
                    {"title", "All about that bass"},
                    {"text", "The entire page text if you wish."},
                    {"url", "/url/to/page"}
                }
            });

            return data;
        }
    }
}
```
>For the full example, please visit here: https://github.com/kgiszewski/UmbracoBookshelf/blob/master/src/Examine/BookshelfExamineDataService.cs


##Config Files##
So let's tell Umbraco/Examine about data service and new seqarcher by editing two config files:

**~/config/ExamineSettings.config**
```xml
<Examine>
  <ExamineIndexProviders>
    <providers>
      ...
      <!-- REGISTER YOUR SIMPLE DATA SERVICE CLASS HERE -->
      <add name="BookshelfIndexer" type="Examine.LuceneEngine.Providers.SimpleDataIndexer, Examine" dataService="MyNamespace.BookshelfExamineDataService,MyDLLname" indexTypes="Bookshelf" />
    </providers>
  </ExamineIndexProviders>
  <ExamineSearchProviders defaultProvider="ExternalSearcher">
    <providers>
      ...
      <!-- REGISTER YOUR SEARCHER HERE -->
      <add name="BookshelfSearcher" type="Examine.LuceneEngine.Providers.LuceneSearcher, Examine" analyzer="Lucene.Net.Analysis.Standard.StandardAnalyzer, Lucene.Net" />
    </providers>
  </ExamineSearchProviders>
</Examine>
```

**~/config/ExamineIndex.config**
```xml
<ExamineLuceneIndexSets>
  ...
  <!-- REGISTER YOUR INDEX HERE -->
  <IndexSet SetName="BookshelfIndexSet" IndexPath="~/App_Data/TEMP/ExamineIndexes/Bookshelf">
    <IndexUserFields>
      <add Name="id" />
      <add Name="book" />
      <add Name="path" />
      <add Name="title" />
      <add Name="text" />
      <add Name="url" />
    </IndexUserFields>
  </IndexSet>
</ExamineLuceneIndexSets>
```

>Much of this information was derived from Shannon Deminick's blog post which you can find here: http://shazwazza.com/post/using-examine-to-index-search-with-any-data-source/ 

##Search##
Nothing special here except make sure you use the correct searcher in your search class that actually performs the search.

So taking [this example](/Chapter 09 - Searching with Examine/02 - Search Results.md), the `ExamineManager` call would actually be the following:

```c#
var searcher = ExamineManager.Instance.SearchProviderCollection["BookshelfSearcher"];
```

Then you would of course have to alter the query in that file to search the new fields like this: https://github.com/kgiszewski/UmbracoBookshelf/blob/master/src/Examine/BookshelfSearcher.cs

##Rebuilding the Index##
Naturally there may be times that you need to have the index rebuilt from your code, to do so, add the following code to a method that gets called whenever you want it rebuilt:
```
ExamineManager.Instance.IndexProviderCollection["NameOfIndexer"].RebuildIndex();
```

That will handle a full rebuild of the index. You can also update individual index entries but for that please see the [Examine documentation](https://github.com/Shazwazza/Examine/wiki).

[<Back 03 - Debugging with Luke](03 - Debugging with Luke.md)