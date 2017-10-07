# Built-in Functionality
Examine has three indexers built-in to Umbraco.  What are indexers?  Indexers are sets of searchable indexes that will return different types of results.  The three built-in indexers are:

* `InternalIndexer` - Used to power the backoffice search (top left).
* `InternalMemberIndexer` - Used to search members.
* `ExternalIndexer` - Used to index your content for public use.  This indexer excludes password protected content and unpublished pages (whereas the `InternalIndexer` includes them)

Configuration for the Examine (including the indexers) can be found on your system in two files:

* `~/config/ExamineSettings.config`
* `~/config/ExamineIndex.config`

For the most part you will only be using the `ExternalSearcher` because you will most likely just be dealing with published content.  In fact, by default Examine will index all fields of all document types and add several fields to identify things like the Umbraco node Id.

In case you need to add another index, you'll need to consult the Umbraco documentation.

The index is built on application start up if the index directories are empty (per index).  The index directories are located at: `~/App_Data/TEMP/ExamineIndexes`.  You can force a re-indexing by deleting one or all of the subfolders in that directory.  The index will rebuild next time when it starts up (so touch the web.config).

The better way to force a rebuilding of the index is to use the Examine dashboard which is located in the Umbraco backoffice on the `Developer` dashboard.

## Examine Dashboard
The examine dashboard is a web GUI that allows you to rebuild the index for a particular indexer and it also provides a test searcher for each index:

![rebuild](assets/examine-rebuild.png)

When testing with the Examine search tool, you can see the fields that Examine is able to see in the Lucene index files:

![search tool](assets/examine-search-tool.png)

## Updating the Indexes
Whenever a node is saved and published, the index files are automatically updated.  If you need to do some direct index manipulation, there are a few events that you can use to hook into the pipeline.  The most used event is probably `GatheringNodeData` and can be implemented like so:

```c#
using Examine;
using Umbraco.Core;

namespace MyNamespace
{
    public class ExamineStuff : ApplicationEventHandler
    {
        protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            base.ApplicationStarted(umbracoApplication, applicationContext);

            ExamineManager.Instance.IndexProviderCollection["ExternalIndexer"].GatheringNodeData += MyGatheringNodeDataMethod;
        }

        private void MyGatheringNodeDataMethod(object sender, IndexingNodeDataEventArgs nodeData)
        {
            nodeData.Fields.Add("myNewField", "someValue");
        }
    }
}
```

After the index is rebuilt or the node is save/published, you can return to the search tool and see that the field is now in the index:

![events](assets/examine-events.png)

## Complex Property Values
Sometimes the property contains complex data and might need to be deserialized into individual fields or munged into one large field.  The example below uses Archetype:

```C#
using System;
using System.Text;
using Archetype.Models;
using Examine;
using Newtonsoft.Json;
using Umbraco.Core;
using Umbraco.Core.Logging;

namespace Demo.Core.Events
{
    public class ExamineMunger : ApplicationEventHandler
    {
        protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            ExamineManager.Instance.IndexProviderCollection["ExternalIndexer"].GatheringNodeData += ExamineMunger_GatheringNodeData;
        }

        private void ExamineMunger_GatheringNodeData(object sender, IndexingNodeDataEventArgs nodeData)
        {
            try
            {
                var sb = new StringBuilder();

                //now let's handle those darn harder property types like the page builder
                if (nodeData.Fields.ContainsKey("modules"))
                {
                    _handlePageBuilderModules(sb, nodeData);
                }

                _updateMungedField(nodeData, sb.ToString());
            }
            catch (Exception ex)
            {
                LogHelper.Info<ExamineMunger>(string.Format("Exception while munging nodeId: {0}", nodeData.NodeId));
                LogHelper.Error<ExamineMunger>(ex.Message, ex);
            }
        }

        private static void _handlePageBuilderModules(StringBuilder sb, IndexingNodeDataEventArgs nodeData)
        {
            var currentModuleAlias = string.Empty;

            try
            {
                if (nodeData.Fields.ContainsKey("modules"))
                {
                    var archetypeValueAsString = nodeData.Fields["modules"];

                    var modules = JsonConvert.DeserializeObject<ArchetypeModel>(archetypeValueAsString);

                    foreach (var module in modules)
                    {
                        currentModuleAlias = module.Alias;

                        switch (module.Alias)
                        {
                            case "richtext":
                                sb.Append(" " + _getSafeString(module, "headline"));
                                sb.Append(" " + _getSafeString(module, "text"));

                                break;
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                LogHelper.Info<ExamineMunger>(string.Format("Exception for current module type: {0}", currentModuleAlias));
                LogHelper.Error<ExamineMunger>(ex.Message, ex);
            }
        }

        //this method gets us a safe string to use without HTML
        private static string _getSafeString(ArchetypeFieldsetModel module, string alias)
        {
            var value = module.GetValue<string>(alias);

            if (value == null)
            {
                return string.Empty;
            }

            return value.StripHtml();
        }

        //this method updates our 'mungedField' that will be used in the searcher later 
        private static void _updateMungedField(IndexingNodeDataEventArgs nodeData, string value)
        {
            if (string.IsNullOrEmpty(value))
            {
                return;
            }

            if (nodeData.Fields.ContainsKey("mungedField"))
            {
                nodeData.Fields["mungedField"] += " " + value;

                LogHelper.Info<ExamineMunger>(nodeData.Fields["nodeName"] + " - Updating...");
            }
            else
            {
                nodeData.Fields.Add("mungedField", value);

                LogHelper.Info<ExamineMunger>(nodeData.Fields["nodeName"] + " - Creating...");
            }
        }
    }
}
```

And when you return to the search tool, you'll now see the new fields each representing an Archetype fieldset:

![complex](assets/examine-complex.png)

[<Back Overview](README.md)

[Next> 02 - Search Results](02%20-%20Search%20Results.md)
