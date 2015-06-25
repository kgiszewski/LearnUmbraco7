#Request Pipeline#

By default, URL's are generated and routed to content based on the content tree hierarchy structure.

The underlying mechanisms at play are grouped into inbound and outbound pipelines.  The inbound pipeline takes a requested URL and 'finds' the content in Umbraco while the outbound pipeline is responsible for generating URL's.

Most developers will be happy to use all of the defaults with maybe a few exceptions added to the [UrlRewriting.config](http://bizmag.local/umbraco/#/UmbracoBookshelf/UmbracoBookshelfTree/file/%252FBooks%252FLearnUmbraco7%252Fz-Appendix%2520D%2520-%2520Config%2520Files%252F02%2520-%2520UrlRewriting.config.md).  However Umbraco has provided us with a way to change the default behavior of both the inbound and outbound pipelines.

This section won't go into great detail because it's already been very well documented by [Stephan Gay](https://twitter.com/zpqrtbnk) of the core team [here](https://our.umbraco.org/documentation/Reference/Request-Pipeline/document/TheUmbraco6RequestPipeline.pdf).

>Note that the link to the PDF above is within the context of Umbraco version 6.

The important take away here is there are ways to be able to route inbound requests to content with your custom code and ways to alter the default URL generation.

[<Back Overview](README.md)

[Next> 02 - Alternate Templates](02 - Alternate Templates.md)