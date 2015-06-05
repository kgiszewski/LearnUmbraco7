#UrlRewriting.config#

This config file is a really for a third-party plugin hosted on GitHub:  https://github.com/aspnetde/UrlRewritingNet.  There is a PDF with more documentation in that repo.

The basic idea is that any entries in this config file is will get executed for each request (pages, images, etc).  This is done through a .NET HTTP handler configured in the `web.config`.

You can add an entry by creating an `<add>` element and using REGEX operators:

```xml
    <add name="category-by-issue"
        virtualUrl="^~/category-by-issue/(\d*)/(.*)"
        rewriteUrlParameter="ExcludeFromClientQueryString"
        destinationUrl="~/category-by-issue/?issueId=$1&amp;categoryName=$2"
        ignoreCase="true" />
```

The above example listens for any URL starting with `/category-by-issue/` followed by a number then a slash then by any text.  If a match is found, you can then rewrite the URL (but hidden from the user/browser) to a completely different URL if desired.

Here's another example:
```xml
    <add name="myurl"
        virtualUrl="^~/foo"
        rewriteUrlParameter="ExcludeFromClientQueryString"
        destinationUrl="~/umbraco/surface/mysurfacecontroller/mymethod"
        ignoreCase="true" />
```

The above entry rewrites any URL starting with `/foo` and executes a surface controller.

[<Back 01 - Web.Config](01 - Web.Config.md)

[Next> 03 - Applications.config](03 - Applications.config.md)