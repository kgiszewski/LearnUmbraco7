# Alternate Templates

Sort of a lesser known feature of Umbraco is the concept of 'alternate templates'.  Alternate templates allow you to take a normal URL like `http://mydomain.local/foo/bar` and append a template name to the end like so `http://mydomain.local/foo/bar/rss` and the content will render with a completely different template.

In order for this to work, you will create a normal page and assign a template to it.  Create a second template and name it `rss` (in this case).

When users navigate to `http://mydomain.local/foo/bar`, the page will render with it's normally assigned template.  When users navigate to`http://mydomain.local/foo/bar/rss`, the same content will render but this time with the alternate template (rss).

This works for any template/content combination except if the template applied to the content isn't appropriate, you could get server errors due to the template trying to access things that may not be on the content node.

>Also note that since a user can do this, keep this in mind as a reminder to not assume any information on a content node is completely hidden from view.

[<Back 01 - Request Pipeline](01%20-%20Request%20Pipeline.md)