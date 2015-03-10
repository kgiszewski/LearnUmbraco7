#Content Section#

The content section is where all of the content (not media) is added to an Umbraco site.   During a fresh install, the only node that will be in your content tree will be the `Recycle Bin` *.  All other pieces of content will need to be defined by a [document type](/Chapter 03 - Document Types, Templates and Data Types/01 - Document Types.md).  The content section also will have a few dashboards by default like: `Get Started` and `Change Password`.

**If you installed a starter kit when you installed Umbraco, you will have some content (and document types) already.*

![Content Section](assets/backoffice.png)

##Content Tree##

The content tree is a hierarchy of document types and each one is affectionately called a node.  By nesting content nodes underneath each other, Umbraco builds the default URL based on the structure.

See how the URL's would be generated below as both a single site or a multi-site install:
 
**Single Site**

    Home  "http://foo.com/"
    |---- About Us "/about-us"
    |---- Contact Us "/contact-us"
   
**Multi-site setup ([see chapter on multi-site configuration](/Chapter 12 - Multisite Configuration))**

    Sites
    |----Site 1
           |---- Home  "http://foo.com/"
                   |---- About Us "/about-us"
                   |---- Contact Us "/contact-us"
    |----Site 2
            |---- Home "http://bar.com/"
                    |---- About Us "/about-us
                    |---- Contact Us "/contact-us
                            |----- "Inquiry Form" /contact-us/inquiry-form

##A content node is the basic content building block in Umbraco##

A content node...

* Represents a single document type
* Will have an icon defined by its document type
* Will have data inputs (property editors) stored in fields as defined by its document type
* May have 0 or more templates associated with it
* May be published or unpublished (denoted by greyed out)
* Can only nest under each other if the parent node allows the child document type
* Don't have to be pages as it is common to use a node as a data holder
* Will be rendered by the RenderMVC controller by default and will return a view  based on what the node has set as a template
* Has an auto- generated tab called `Properties` which handles a few things like: 
    * Display node ID
    * Template selection
    * Auto publish/unpublish by date
    * The URL to the node from the users perspective
    * Document type identification

##Controlled Experience##
Based on node permissions, the user can be restricted as to what they can see.  Typically a users is assigned a content root node in the [Users Section](/Chapter 02 - Backoffice Orientation/05 - Users Section.md) and they will only be able to navigate underneath (and including) the node selected.  You can also assign other restrictions as well (i.e. context menu options).

##Context Menu##

Each node can be right-clicked on and a set of context menus options will be presented to the user.

![Context Menu](assets/context-menu.png)

An administrator can limit the context menu options based on the user as well as a developer can add [custom actions](/Chapter 16 - Custom Sections, Trees and Actions).