#Custom IAction#

Umbraco provides most of the actions you'll ever need, but if you ever find the need to create your own, you'll simply just have to implement the `IAction` interface:

```
using System;
using umbraco.interfaces;

namespace MyNamespace
{
    public class MyIAction : IAction
    {

        public string Alias
        {
            get { throw new NotImplementedException(); }
        }

        public bool CanBePermissionAssigned
        {
            get { throw new NotImplementedException(); }
        }

        public string Icon
        {
            get { throw new NotImplementedException(); }
        }

        public string JsFunctionName
        {
            get { throw new NotImplementedException(); }
        }

        public string JsSource
        {
            get { throw new NotImplementedException(); }
        }

        public char Letter
        {
            get { throw new NotImplementedException(); }
        }

        public bool ShowInNotifier
        {
            get { throw new NotImplementedException(); }
        }
    }
}
```

You can then select your `IAction` in your own code:

```c#
using System;
using umbraco.BusinessLogic.Actions;
using umbraco.interfaces;
using Umbraco.Web.Trees;
using Umbraco.Web.Models.Trees;

namespace MyNamespace
{
    public class MyTreeController : TreeController
    {
        protected override MenuItemCollection GetMenuForNode(string id, System.Net.Http.Formatting.FormDataCollection queryStrings)
        {
            var menu = new MenuItemCollection();

            //examine the node id, and only add these actions for some items
            if (!id.EndsWith(".md"))
            {
                menu.Items.Add<MyAction>("Custom Action");
            }

            //add these to all
            menu.Items.Add<ActionMove>("Rename");
            menu.Items.Add<ActionDelete>("Delete");

            return menu;
        }

        protected override TreeNodeCollection GetTreeNodes(string id, System.Net.Http.Formatting.FormDataCollection queryStrings)
        {
            throw new NotImplementedException();
        }
    }

    public class MyAction : IAction
    {
        public string Alias
        {
            get { throw new NotImplementedException(); }
        }

        public bool CanBePermissionAssigned
        {
            get { throw new NotImplementedException(); }
        }

        public string Icon
        {
            get { throw new NotImplementedException(); }
        }

        public string JsFunctionName
        {
            get { throw new NotImplementedException(); }
        }

        public string JsSource
        {
            get { throw new NotImplementedException(); }
        }

        public char Letter
        {
            get { throw new NotImplementedException(); }
        }

        public bool ShowInNotifier
        {
            get { throw new NotImplementedException(); }
        }
    }
}
```

>I've created an `IAction` in a side project that adds 'move' functionality to the `Settings->Document Type` node items.  You can find it here: https://github.com/kgiszewski/DocumentTypeMover

[<Back 03 - Custom Tree Menu Actions](03 - Custom Tree Menu Actions.md)