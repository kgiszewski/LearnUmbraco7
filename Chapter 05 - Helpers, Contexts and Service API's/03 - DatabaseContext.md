#DatabaseContext#

The `DatabaseContext` holds a reference to the underlying SQL provider.  The SQL provider can be MSSQL, MySQL and SQL CE.  Under the covers, the Database context is using PetaPoco which is a micro-ORM.  For more information regarding PetaPoco, please visit this: http://www.toptensoftware.com/petapoco/.

You will only use the `DatabaseContext` object if you need to do database operations outside of the Umbraco API.  You should not be using this context to alter the core tables, please use the built-in `IContent` or `IPublishedContent` API's if that is your aim.

##Sample Usage##
>The examples that follow work with MSSQL but may not work with the other providers.

PetaPoco requires a model (the POCO) to represent a row in a table.  So let's create a model for that purpose.  

Our sample model below will represent a page view for a wiki article in a previous created table called `Wiki_Views`.  It has four columns and it's primary key column is `Id`.

```
using System;
using Umbraco.Core.Persistence;

namespace MyNamespace
{
    //Which table?
    [TableName("wiki_views")]
    //What is the primary key column?
    [PrimaryKey("id")]
    public class WikiView
    {
        public long Id { get; set; }
        public long NodeId { get; set; }
        public int Views { get; set; }
        public DateTime LastView { get; set; }
    }
}
```

Let's create a helper class to do some of the CRUD operations:

```
using System;
using System.Collections.Generic;
using System.Linq;
using Umbraco.Core.Models;
using Umbraco.Core;
using Umbraco.Web;

namespace MyNamespace
{
    public class WikiHelper
    {
       //shortcut to get the databasecontext
        public DatabaseContext DbContext 
        {
            get
            {
                return ApplicationContext.Current.DatabaseContext;
            }
        }

        //add a view
        public WikiView AddView(IPublishedContent content)
        {
            if (content == null)
            {
                return null;
            }

            //go see if we are already tracking this item
            var view = _getViewById(content.Id);
 
            //do an update if null
            if(view != null)
            {
                view.LastView = DateTime.Now;
                view.Views++;
                DbContext.Database.Update(view);
            }
            else
            {
                //perform an insert
                DbContext.Database.Insert(new WikiView()
                {
                    LastView = DateTime.Now,
                    NodeId = content.Id,
                    Views = 1
                });
            }

            return _getViewById(content.Id);
        }

        //perform a simple single row select
        private WikiView _getViewById(int nodeId)
        {
            return DbContext.Database.SingleOrDefault<WikiView>(@"
                SELECT *
                FROM Wiki_Views
                WHERE nodeID = @0
            ", nodeId);
        }
    }
}
```

Then finally let's call the helper from a controller, view or wherever:

```
    var helper = new WikiHelper();
    
    var view = helper.AddView(Model.Content);
```

While the sampling above is not all inclusive, you can also perform transactions and you should consult the main PetaPoco website for further help.

[<Back 02 - UmbracoContext](02 - UmbracoContext.md)

[Next> 04 - UmbracoHelper](04 - UmbracoHelper.md)