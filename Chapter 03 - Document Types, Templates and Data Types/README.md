#Overview#

Document types, templates and data types are the structures that your website will ultimate be built with.

Data types are the inputs that your editors will use to input data.

Document types are collections of data types that can represent either a page or just data.

Templates is where your markup will live.  In the MVC world of Umbraco the view is provided with a RenderMVC model (by default).  The RenderMVC model has a property called `Content` of type `IPublishedContent`.  From IPublishedContent you can access all of your data types living on your document type.  You can override the default model by hijacking the default controller and providing a custom model.