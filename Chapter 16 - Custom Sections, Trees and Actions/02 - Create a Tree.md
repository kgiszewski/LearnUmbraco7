#Create a Tree#
Creating a tree in Umbraco can be fairly tricky because of some of the references are hard to grasp.

There are three main types of trees found in Umbraco:

* Content trees
* File system trees
* Other trees

At a minimum, you'll need to inherit from one of the provided `TreeController` classes like so:

```c#
using System;
using System.Net.Http.Formatting;
using Umbraco.Web.Models.Trees;
using Umbraco.Web.Trees;

namespace UmbracoBookshelf.Controllers
{
    public class MyTreeController : TreeController
    {
        protected override MenuItemCollection GetMenuForNode(string id, FormDataCollection queryStrings)
        {
            throw new NotImplementedException();
        }

        protected override TreeNodeCollection GetTreeNodes(string id, FormDataCollection queryStrings)
        {
            throw new NotImplementedException();
        }
    }
}

```

The method `MenuItemCollection()` will be where you'll put context menu items relevant to the node in question (we'll look at it in the next section).  The `GetTreeNodes()` method determines which nodes appear under the provided node identified by the `id` parameter.

##GetTreeNodes()##
The trick to this method is to only return the nodes that should be under the node with the provided `id`.  So an example might be:

```C#
protected override TreeNodeCollection GetTreeNodes(string id, FormDataCollection queryStrings)
{
    var nodes = new TreeNodeCollection();

    nodes.Add(CreateTreeNode("123", "456", queryStrings, "Some name to be shown"));
    nodes.Add(CreateTreeNode("789", "456", queryStrings, "Some other name to be shown"));

    return nodes;
}
```

Making the nodes show the content you want dynamically will probably be more useful.

##Trees.config##
Similar to our `applications.config` we need to update our `~/config/trees.config` file to register our tree for our section.

First let's look at our config file with the some of the core stuff included along with our new tree:

```xml
<trees>
...
  <add initialize="true" sortOrder="3" alias="prevaluesource" application="forms" title="Prevalue sources" iconClosed="icon-folder" iconOpen="icon-folder-open"
  	type="Umbraco.Forms.Web.Trees.PreValueSourceTreeController, Umbraco.Forms.Web"/>
  <add initialize="true" sortOrder="0" alias="UmbracoBookshelfTree" application="UmbracoBookshelf" title="Umbraco Bookshelf" iconClosed="icon-folder"
  	iconOpen="icon-folder-open" type="UmbracoBookshelf.Controllers.UmbracoBookshelfTreeController, UmbracoBookshelf"/>
</trees>
```
>Same thing as the `application.config` applies here.  We'll use an attribute to add the tree registration.  You can de-register it here by removing the line manually.

##FileTreeController##
Our bookshelf project will use another type of built-in tree controller, the `FileTreeController` like so:


```
using System;
using System.IO;
using System.Linq;
using System.Net.Http.Formatting;
using umbraco.BusinessLogic.Actions;
using Umbraco.Core.IO;
using Umbraco.Web.Models.Trees;
using Umbraco.Web.Mvc;
using Umbraco.Web.Trees;

namespace UmbracoBookshelf.Controllers
{
    [PluginController("UmbracoBookshelf")]
    [Umbraco.Web.Trees.Tree("UmbracoBookshelf", "UmbracoBookshelfTree", "Umbraco Bookshelf", iconClosed: "icon-folder")]
    public class UmbracoBookshelfTreeController : FileSystemTreeController
    {
        protected override string FilePath
        {
            get { return Helpers.Constants.ROOT_DIRECTORY; }
        }

        protected override string FileSearchPattern
        {
            get { return "*" + Helpers.Constants.MARKDOWN_FILE_EXTENSION; }
        }

        //this portion will be covered in the next section
        protected override MenuItemCollection GetMenuForNode(string id, FormDataCollection queryStrings)
        {
            var menu = new MenuItemCollection();

            if (!id.EndsWith(Helpers.Constants.MARKDOWN_FILE_EXTENSION))
            {
                menu.Items.Add<ActionNew>("Create");
                menu.Items.Add<ActionRefresh>("Reload Nodes");
            }
            
            menu.Items.Add<ActionMove>("Rename");
            menu.Items.Add<ActionDelete>("Delete");

            return menu;
        }

        protected override TreeNodeCollection GetTreeNodes(string id, FormDataCollection queryStrings)
        {
            var nodes = new TreeNodeCollection();

            nodes.AddRange(getNodes(id, queryStrings));

            return nodes;
        }

        private string getWebPath(string mappedPath)
        {
            var urlRoot = new Uri(IOHelper.MapPath("~") + "/");
            var path = urlRoot.MakeRelativeUri(new Uri(mappedPath)).ToString();        

            //normal filesystem placement
            path = path.Replace(".." + Helpers.Constants.ROOT_DIRECTORY , "");

            //fixup virtual relative paths
            path = path.Replace("/..", "").Replace("..","");

            //both virtual and normal
            path = path.Replace("/", "%2F");

            return path;
        }

        private TreeNodeCollection getNodes(string id, FormDataCollection queryStrings)
        {
            var orgPath = "";
            var path = "";

            if (!string.IsNullOrEmpty(id) && id != "-1")
            {
                orgPath = id;
                path = IOHelper.MapPath(FilePath + "/" + orgPath);
                orgPath += "/";
            }
            else
            {
                path = IOHelper.MapPath(FilePath);
            }

            var dirInfo = new DirectoryInfo(path);
            var dirInfos = dirInfo.GetDirectories();

            var nodes = new TreeNodeCollection();

            foreach (var dir in dirInfos)
            {
                var hasChildren = dir.GetFiles().Any(x => x.Name.EndsWith(Helpers.Constants.MARKDOWN_FILE_EXTENSION)) || dir.GetDirectories().Length > 0;

                if (hasChildren && (dir.Attributes & FileAttributes.Hidden) == 0)
                {
                    var node = CreateTreeNode(orgPath + dir.Name, orgPath, queryStrings, dir.Name, "icon-folder", hasChildren);

                    node.RoutePath = "/UmbracoBookshelf/UmbracoBookshelfTree/folder/" + getWebPath(dir.FullName);

                    if (node != null)
                        nodes.Add(node);
                }
            }

            //this is a hack to enable file system tree to support multiple file extension look-up
            //so the pattern both support *.* *.xml and xml,js,vb for lookups
            var allowedExtensions = new string[0];
            var filterByMultipleExtensions = FileSearchPattern.Contains(",");
            FileInfo[] fileInfo;

            if (filterByMultipleExtensions)
            {
                fileInfo = dirInfo.GetFiles();
                allowedExtensions = FileSearchPattern.ToLower().Split(',');
            }
            else
                fileInfo = dirInfo.GetFiles(FileSearchPattern);

            foreach (var file in fileInfo)
            {
                if ((file.Attributes & FileAttributes.Hidden) == 0)
                {
                    if (filterByMultipleExtensions && Array.IndexOf<string>(allowedExtensions, file.Extension.ToLower().Trim('.')) < 0)
                        continue;

                    var node = CreateTreeNode(orgPath + file.Name, orgPath, queryStrings, file.Name, "icon-document", false);

                    node.RoutePath = "/UmbracoBookshelf/UmbracoBookshelfTree/file/" + getWebPath(file.FullName);

                    if (node != null)
                        nodes.Add(node);
                }
            }

            return nodes;
        }
    }
}
```

There is a lot going on in this file and most of it has to do with the business logic of how the project works.  Hopefully this gives you an idea of what you can do with your file tree.

[<Back 01 - Create a Section](01 - Create a Section.md)

[Next> 03 - Custom Tree Menu Actions](03 - Custom Tree Menu Actions.md)