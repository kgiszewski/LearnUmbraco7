# Document Types

As stated before, a document type (sometimes called content type) represents an entity in Umbraco.  It has the following facets:

* It has a specific icon (that shows in the content tree)
* It can have one or more templates associated
* It can have one or more tabs that hold one or more data types
* It controls which children document types can be added underneath in the content tree
* It is identified by an alias
* Can use inheritance or composition

The way you setup your document types can make or break your editors experience and overall design of your website.  They are managed via `Settings->Document Types` in the backoffice.

In general you should follow these guidelines:

* One document type for one template
* Split up associated data types with tabs
* Prefer using compositions rather than inheritance
* Consider creating a base SEO and\or common doc types that includes properties like `keywords`, `description`, `hide from search`, etc.
* Consider creating document types for entities not meant to be pages (i.e. something that is picked by a mult-node tree picker). These types will have no templates.

A lot has changed in Umbraco (a good thing) and here's a video for version 7.7.x to fast-forward you along: https://www.youtube.com/watch?v=EopnnkIaiRI


[<Back Overview](README.md)

[Next> 02 - Data Types](02%20-%20Data%20Types.md)
