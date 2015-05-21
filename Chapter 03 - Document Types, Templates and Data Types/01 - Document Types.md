#Document Types#

As stated before, a document type (sometimes called content type) represents an entity in Umbraco.  It has the following facets:

* It has a specific icon (that shows in the content tree)
* It can have one or more templates associated
* It can have one or more tabs that hold one or more data types
* It controls which children document types can be added underneath in the content tree
* It is identified by an alias

The way you setup your document types can make or break your editors experience and overall design of your website.  They are managed via `Settings->Document Types` in the backoffice.

In general you should follow these guidelines:

* One document type for one template
* Split up associated data types with tabs
* Use inheritance where document types are similar with respect to data types
* Consider creating a base document type to which everything inherits
* Consider creating a base web page type that includes properties like `keywords`, `description`, `hide from search`, etc.
* Consider creating a base document type for entities not meant to be pages (i.e. something that is picked by a mult-node tree picker).

##Info Tab##
The `Info` tab is where you'll set the document types human and machine (alias) name.  You'll also set the icon that represents this document type in the content tree along with zero or more templates that are associated with this content type.

##Structure Tab##
The structure tabs is where you'll indicate what document types can be created underneath this type of document while in the `Content` section.  You can also indicate if this document is able to be created at the very root of the content section along with setting `List View` for child nodes instead of listing children in the tree.

> Please note that if a document type is meant to be a single instance (i.e. the home page), be sure to come back to this section after the first node is created to disable further instances of a particular document type.

##Generic Properties##
This is where you'll define tabs and data types that will appear on a document type while editing in the `Content` section.  You should avoid putting items on the `Generic Properties` tab, instead create a tab named something like `Content`.  

>Below is a sample document type setup that shows everything inherits off of one document type `base`.  Then it branches into entities that represent pages and entities that represent just pieces of data.  The items under `Base Data` will have no templates associated as they are not meant to represent viewable pages.

![Doctypes](assets/doctypes2.png)

##Tabs Tab##
Here is where you'll manage your tabs for your documents' data types.  Tabs are inherited, so often you'll see a `Content` tab placed on a base document type up the chain.

[<Back Overview](README.md)

[Next> 02 - Data Types](02 - Data Types.md)