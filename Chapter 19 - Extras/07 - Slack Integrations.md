# Slack Integrations

Use Slack? In this section we will cover how to send a message to Slack from Umbraco whenever an unhandled exception occurs.

It will involve four easy steps:

* Create a custom Slack integration (in Slack)
* Create a model to represent the data sent to Slack
* Extend the UmbracoApplication class
* Update your Global.asax

## Create your custom Slack integration
Simply go to your Slack application and add a new integration. Select `Inbound` integration. Fill out the required fields. We will need the hook URL that Slack will provide you.

## Create a model

Create a class that handles the variables we'll be sending to Slack like so:

```c#
using Newtonsoft.Json;

namespace MyNamespace
{
    public class SlackSaySomethingModel
    {
        [JsonProperty(PropertyName = "username")]
        public string Username { get; set; }

        [JsonProperty(PropertyName = "icon_emoji")]
        public string Emoji { get; set; }

        [JsonProperty(PropertyName = "icon_url")]
        public string IconUrl { get; set; }

        [JsonProperty(PropertyName = "channel")]
        public string Channel { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }
    }
}
```

## Create a new class that extends UmbracoApplication
This class extends the `UmbracoApplication` which has already extended the usual `Global.asax.cs` file. We do this to hook into the `Application_Error` method as an extension point to grab unhandled errors. I've created two helper methods that can be used to facilitate the actual sending of the data.

```c#
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;
using Umbraco.Web;

namespace MyNamespace
{
    public class MyApplication : UmbracoApplication
    {
        protected new void Application_Error(object sender, EventArgs e)
        {
            base.Application_Error(sender, e);

            var lastError = Server.GetLastError();

            if (lastError != null)
            {
                _saySomething(lastError.Message, "UmbracoBot", "http://slackurl-goes-here", "#mychannel", ":my_emoji:");   
            }
        }

        private static string _saySomething(string text, string username, string url, string channel = "", string emoji = "", string iconUrl = "")
        {
            var slackPost = new SlackSaySomethingModel()
            {
                Username = username,
                Channel = channel,
                Emoji = emoji,
                IconUrl = iconUrl,
                Text = text
            };

            var postBody = JsonConvert.SerializeObject(slackPost);

            var content = new StringContent(postBody, Encoding.UTF8, "application/json");

            return _post(url, content).Result;
        }

        private async static Task<string> _post(string url, HttpContent content)
        {
            using (var client = new HttpClient())
            {
                var response = await client.PostAsync(url, content).ConfigureAwait(false);

                return await response.Content.ReadAsStringAsync();
            }
        }
    }
}
```

## Update your Global.asax file

Finally you just need to tell Umbraco to use your new class by updating the `Global.asax` file located on your web root like so:

```
<%@ Application Inherits="MyNamespace.MyApplication" Language="C#" %>
          
```

## Summary
Be sure to update the URL and channel name in the code above to use the URL that Slack provided you. You can use any of the emoji's and don't forget to give your bot a great name!

>Pro tip, your channel should have a hash tag on it i.e. `#umbraco` or you can send it to a user `@someuser`

>Pro tip #2, as Warren Buckely points out, be sure to filter/ignore to desired taste otherwise it could get noisy in that room.
