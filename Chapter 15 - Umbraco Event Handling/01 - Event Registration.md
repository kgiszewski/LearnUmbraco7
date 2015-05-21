#Event Registration#

You have three opportunities at different points in time to register an event from your code.  You will register these events by extending the `ApplicationEventHandler` class like so:

```c#
using Umbraco.Core;

namespace MyNamespace
{
    public class MyClass : ApplicationEventHandler
    {
        protected override void ApplicationInitialized(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            base.ApplicationInitialized(umbracoApplication, applicationContext);

            //register an event here
        }

        protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            base.ApplicationStarted(umbracoApplication, applicationContext);

            //or here

            //this is the usual spot
        }

        protected override void ApplicationStarting(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            base.ApplicationStarting(umbracoApplication, applicationContext);
            //or here
        }
    }
}
```

A typical use of the `ContentService.Published` and the `ContentService.Publishing` events are like so:

```c#
using Umbraco.Core;
using Umbraco.Core.Events;
using Umbraco.Core.Logging;
using Umbraco.Core.Models;
using Umbraco.Core.Publishing;
using Umbraco.Core.Services;

namespace MyNamespace
{
    public class MyClass : ApplicationEventHandler
    {
        protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            base.ApplicationStarted(umbracoApplication, applicationContext);

            ContentService.Publishing += MyCodeThatWillCancelPublishing;
            ContentService.Published += MyCustomCodeToRun;

            OneTimeRunCode();
        }

        private void MyCustomCodeToRun(IPublishingStrategy sender, PublishEventArgs<IContent> args)
        {
            //runs when one or more nodes are published
            foreach (IContent item in args.PublishedEntities)
            {
                //notice the type is IContent here
                LogHelper.Info<MyClass>(item.Name + " was published.");
            }
        }

        private void MyCodeThatWillCancelPublishing(IPublishingStrategy sender, PublishEventArgs<IContent> args)
        {
            if (true)
            {
                args.Cancel = true;
            }
        }

        private void OneTimeRunCode()
        {
            //runs once
        }
    }
}
```

The list of events is pretty long so please checkout the official documentation.

[<Back Overview](README.md)