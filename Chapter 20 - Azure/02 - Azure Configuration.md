#Azure Configuration#
The following bits are things you will need to consider or setup if you'd like to implement the topology from section 1.

##DB Connection String
You can keep your DB connection string details out of your repo by configuring the linked DB in Azure properly.  In fact, if you just change the connection string name (under Configure) to `UmbracoDbDSN`, it'll automatically transform your web.config to use the linked DB.

##Web.config Transforms
If you aren't familiar with web.config transforms, you're truly missing out.  Transforms allow you to specify certain settings in your web.config based on environment.  I.e. if you want an `appSetting` to have one value when working locally and another when in production, then transforms are your friend.

So for our setup, we will want to transform a few things, let's look at them by environment:

###Local
Locally we will keep everything normal, not using Azure for anything.  Therefore nothing new here.

###Azure Prod/Preprod
For these two environments, we'll use a single transform.  Create a transform called `Web.Azure-Prod.config`, we will change the file system provider and the Image Processor cache setup (more on these in later sections).

###Azure Dev
We will retain the same settings as the local environment, but you could create a `Web.Azure-Dev.config` and do something specific there.

##Get Azure to Use a Specific Transform
To get Azure to use a specific transform on a particular slot, go to the `configure` area for your slot and add an app setting:

`SCM_BUILD_ARGS` with value of `-p:PublishProfile=Azure-Prod`

##Azure Services
You need to sign up for these services as well:
* CDN - For Image processor caching
* Azure Storage - For Image Processor caching and media

[<Back 01- Slots](01- Slots.md)

[Next> 03 - File System Provider](03 - File System Provider.md)