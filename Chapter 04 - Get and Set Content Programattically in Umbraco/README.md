#Overview#

![5838817600_1da1563821_o.jpg](assets/5838817600_1da1563821_o.jpg)
>Photo by: Doug Robar

Umbraco is a very easy to use content management system.  Getting data in Umbraco is a matter of deciding of whether it should be sourced from published content or from the backoffice directly.

Published content is delivered through the `IPublishedContent`class.  Typically you will encounter this class when working with views.  `IPublishedContent` is cached and is `read only`.  This is where you should be getting your data if you need the latest published copy.

The backoffice data is delivered via the `IContent` class.  This is where you'll want to be if you need to do any sort of manipulating of the data.  This class is much slower (compared to `IPublishedContent`) because of the requisite database hits that will occur when saving your changes.  This is also the class you'll want to be working with if you need to do any of the following:

* Access older versions of the node
* Save
* Publish
* Create new nodes

>Remember, if you're just needing to get published content, use the `IPublishedContent` class family.  If you need to do any other CRUD operations, use `IContent`.

[Next> 01 - IPublishedContent](01 - IPublishedContent.md)