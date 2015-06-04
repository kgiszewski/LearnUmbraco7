#Web.config#

You have a main `Web.config` file located in your web root that will be a file you should at least be somewhat familiar with.  This section will attempt to cover some of the more interesting bits.

##AppSettings##
```xml
  <appSettings>
    <add key="umbracoConfigurationStatus" value="x.x.x" />
    <add key="umbracoReservedUrls" value="~/config/splashes/booting.aspx,~/install/default.aspx,~/config/splashes/noNodes.aspx,~/VSEnterpriseHelper.axd" />
    <add key="umbracoReservedPaths" value="~/umbraco,~/install/" />
    <add key="umbracoPath" value="~/umbraco" />
    <add key="umbracoHideTopLevelNodeFromPath" value="true" />
    <add key="umbracoUseDirectoryUrls" value="true" />
    <add key="umbracoTimeOutInMinutes" value="200000" />
    <add key="umbracoDefaultUILanguage" value="en" />
    <add key="umbracoUseSSL" value="false" />
    <add key="ValidationSettings:UnobtrusiveValidationMode" value="None" />
    <add key="webpages:Enabled" value="false" />
    <add key="enableSimpleMembership" value="false" />
    <add key="autoFormsAuthentication" value="false" />
    <add key="log4net.Config" value="config\log4net.config" />
  </appSettings>
```

This part of the web.config is where you'll find ways to control certain behaviors of Umbraco.  To modify the behaviors, simply update the value and save.
>Note that any changes saved automatically trigger a application pool restart.

Let's walk through a few of these:

* **umbracoConfigurationStatus** - The current version of your Umbraco build, it should match the version number found in the backoffice (click the question mark bottom left)
* **umbracoReservedUrls** - URL's that we tell Umbraco to ignore
* **umbracoReservedPaths** - Entire paths we tell Umbraco to ignore
* **umbracoPath** - Where to find the Umbraco code, changing this could potentially harden your site but may make things tricky to update/install
* **umbracoHideTopLevelNodeFromPath** - Fiddling with this allows for the root node to respond to `/` versus `/mypage`
* **umbracoTimeOutInMinutes** - Set this to a larger value in milliseconds to avoid having the backoffice timeout
* **umbracoUseSSL** - Set to true and the backoffice will force the user to use SSL (recommended)

>Please consult the official Umbraco docs for further details

##ConnectionStrings##
This is where you'll tell Umbraco where the database lives.  If you want to incorporate another database connection string, you can just add another `<add>` element to bring in another database resource.

```xml
  <connectionStrings>
    <remove name="umbracoDbDSN" />
    <add name="umbracoDbDSN" connectionString="server=.\SQLEXPRESS;database=myservername;user id=sa;password=1234" providerName="System.Data.SqlClient" />
    <!-- Important: If you're upgrading Umbraco, do not clear the connection string / provider name during your web.config merge. -->
  </connectionStrings>
```

##SMTP##
If you send emails from Umbraco, be sure to setup the SMTP.  You can use http://sendgrid.net for low volume free SMTP or high volume pay services.

```
  <system.net>
    <mailSettings>
      <smtp>
        <network host="smtp.sendgrid.net" password="1234" userName="foo" port="587" />
      </smtp>
    </mailSettings>
  </system.net>
```

##System.Web##

This bit is useful for turning custom errors on or off, setting specific URL's on errors and setting values for max upload size and max time a page can execute.

```
  <system.web>
    <customErrors mode="Off" defaultRedirect="/server-error/">
      <error statusCode="404" redirect="/page-not-found/" />
      <error statusCode="500" redirect="/server-error/" />
    </customErrors>
    <httpRuntime requestValidationMode="2.0" enableVersionHeader="false" targetFramework="4.5" maxRequestLength="26214400" executionTimeout="3600" />
  </system.web>
```

##Membership##
Out of the box, Umbraco is using it's own membership providers for both users and members.  This next section is where you'd add you own providers if you want to alter the login process:

```xml
    <membership defaultProvider="UmbracoMembershipProvider" userIsOnlineTimeWindow="15">
      <providers>
        <clear />
        <add name="UmbracoMembershipProvider" type="MyApp.UmbracoExtensions.Shared.MembershipProviders.MemberMembershipProvider, MyApp.UmbracoExtensions" minRequiredNonalphanumericCharacters="0" minRequiredPasswordLength="4" useLegacyEncoding="true" enablePasswordRetrieval="false" enablePasswordReset="true" requiresQuestionAndAnswer="false" defaultMemberTypeAlias="Member" passwordFormat="Hashed" />
        <!--<add name="UmbracoMembershipProvider" type="Umbraco.Web.Security.Providers.MembersMembershipProvider, Umbraco" minRequiredNonalphanumericCharacters="0" minRequiredPasswordLength="4" useLegacyEncoding="true" enablePasswordRetrieval="false" enablePasswordReset="true" requiresQuestionAndAnswer="false" defaultMemberTypeAlias="Member" passwordFormat="Hashed" />-->
        <!--<add name="UsersMembershipProvider" type="Umbraco.Web.Security.Providers.UsersMembershipProvider, Umbraco" minRequiredNonalphanumericCharacters="0" minRequiredPasswordLength="4" useLegacyEncoding="true" enablePasswordRetrieval="false" enablePasswordReset="true" requiresQuestionAndAnswer="false" passwordFormat="Hashed" />-->
        <add name="UsersMembershipProvider" type="MyApp.UmbracoExtensions.Shared.MembershipProviders.UserMembershipProvider, MyApp.UmbracoExtensions" minRequiredNonalphanumericCharacters="0" minRequiredPasswordLength="4" useLegacyEncoding="true" enablePasswordRetrieval="false" enablePasswordReset="true" requiresQuestionAndAnswer="false" passwordFormat="Hashed" />
      </providers>
    </membership>
```

##System.WebServer##

Depending on the version of IIS you're running, you may need to update this section for custom errors:

```xml
  <system.webServer>
    
    <httpErrors errorMode="Custom">
      <remove statusCode="404" subStatusCode="-1" />
      <error statusCode="404" prefixLanguageFilePath="" path="/page-not-found/" responseMode="ExecuteURL" />
      <remove statusCode="500" subStatusCode="-1" />
      <error statusCode="500" prefixLanguageFilePath="" path="/server-error/" responseMode="ExecuteURL" />
    </httpErrors>
  </system.webServer>
