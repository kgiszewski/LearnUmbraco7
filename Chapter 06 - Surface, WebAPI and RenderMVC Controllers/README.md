#Overview#

In Umbraco, Surface, RenderMVC and WebAPI controllers are responsible for deliver data (models) to clients via their templates (views).

By default the `RenderMvcController` delivers content (model) with a view (template) to the client.  This can be overridden globally or by document type/template.

`SurfaceController`'s are what you'll use for form submissions or quick web services that may return views, test, JSON or XML.

Finally, `UmbracoApiController`'s allow you to use WebAPI with Umbraco services and contexts made available to you.