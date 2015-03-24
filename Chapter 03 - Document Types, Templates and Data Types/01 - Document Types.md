#Document Types#

As stated before, a document type represents an entity in Umbraco.  It has the following facets:

* It has a specific icon (that shows in the content tree)
* It can have one or more templates associated
* It can have one or more tabs that hold one or more data types
* It controls which children document types can be added underneath in the content tree
* It is identified by an alias

The way you setup your document types can make or break your editors experience and overall design of your website.

In general you should follow these guidelines:

* One document type for one template
* Split up associated data types with tabs
* Use inheritance where document types are similar with respect to data types
* Consider creating a base document type to which everything inherits
* Consider creating a base web page type that includes properties like `keywords`, `description`, `hide from search`, etc.
* Consider creating a base document type for entities not meant to be pages (i.e. something that is picked by a mult-node tree picker).