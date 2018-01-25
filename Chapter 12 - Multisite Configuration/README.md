# Overview

![13391452355_aed8eb7c54_o.jpg](assets/13391452355_aed8eb7c54_o.jpg)
>Photo by Doug Robar

Setting up Umbraco to handle more than one website is easy to do.  Below is a list of things to consider when setting up a multisite install:

* Create a `Site` document type (or whatever you name it) that will reside on root of your content section for each of your sites.
* Use relative paths when using data types such as the multinode tree picker.
* Create a `Home` page under your `Site` node and all pages as children under the `Home` node.
* Assign a domain to each `Home` node.
* Consider creating a `Site Settings` document type to hold settings for each website.

Here is a video about a sample install, you should give it watch if visuals are your thing: https://www.youtube.com/watch?v=0H2o_mwhRhI&t=19s

![multisite](assets/multisite.png)

[Next 01> - Domain Setup](01%20-%20Domain%20Setup.md)
