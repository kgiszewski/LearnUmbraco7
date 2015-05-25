#Custom Membership Providers
One great way to extend Umbraco is to use custom membership providers.  In the examples below, we will use Umbraco to manage everything except checking a backoffice users and frontend users (members) passwords.

##The Idea
The idea is that when a user hits the Umbraco login screen for the backoffice, we want the user to be able to enter in an LDAP password.  Umbraco will still control access to sections, starting nodes and the like.  We will also use the same approach for members logging in to the frontend.  This setup is great when you just need to manage access and not to manage passwords and the overall LDAP.

##Create a Classes to Check Password with LDAP
We need a class to authenticate to the server:
```c#
using System;
using System.DirectoryServices.Protocols;
using System.Globalization;
using System.Net;
using System.Security.Permissions;

namespace MyNamespace.Ldap
{
    public class LdapServer
    {
        public LdapServer(string hostName, string baseDn, string filter)
        {
            this.HostName = hostName;
            this.BaseDn = baseDn;
            this.Filter = filter;
        }

        public string HostName { get; private set; }
        public string BaseDn { get; set; }
        public string Filter { get; set; }
        public LdapUser User { get; private set; }

        [SecurityPermission(SecurityAction.Demand)]
        public LdapUser Authenticate(string userName, string password)
        {
            /* you will need to adjust for your specific LDAP connection */
            using (var ldap = new LdapConnection(new LdapDirectoryIdentifier(this.HostName)))
            {
                ldap.SessionOptions.ProtocolVersion = 3;
                ldap.SessionOptions.SecureSocketLayer = true;

                ldap.AuthType = AuthType.Anonymous;
                ldap.Bind();

                var dn = _getDn(ldap, userName);

                if (dn != null)
                {
                    ldap.AuthType = AuthType.Basic;
                    ldap.Bind(new NetworkCredential(dn, password));
                    return User;
                }

                return null;
            }
        }

        private string _getDn(LdapConnection ldap, String userName)
        {
            var request = new SearchRequest(this.BaseDn, string.Format(CultureInfo.InvariantCulture, this.Filter, userName), SearchScope.Subtree);
            var response = (SearchResponse)ldap.SendRequest(request);

            if (response.Entries.Count == 1)
            {
                User = new LdapUser(response.Entries[0]);
                return response.Entries[0].DistinguishedName;
            }

            return null;
        }
    }
}
```

Next we need a model for the User, please add/modify fields as needed:
```c#
using System.Collections;
using System.DirectoryServices.Protocols;

namespace MyNamespace.Ldap
{
    public class LdapUser
    {
        public string Uid { get; private set; }

        internal LdapUser(SearchResultEntry entry)
        {
            foreach (DictionaryEntry attr in entry.Attributes)
            {
                var name = attr.Key.ToString().ToUpperInvariant();
                var values = (DirectoryAttribute)attr.Value;

                switch (name)
                {
                    case "UID": 
                        this.Uid = values[0].ToString();
                        break;
                }
            }
        }
    }
}
```
So far we're only using regular .NET pieces, next we'll create membership providers that alter how Umbraco handles authentication.  Let's create the `User` provider that overrides the backoffice login:

```c#
using System;
using System.Web.Security;
using Umbraco.Core.Logging;
using Umbraco.Web.Security.Providers;
using MyNamespace.Ldap;

namespace MyNamespace
{
    public class MyUserMembershipProvider : UsersMembershipProvider
    {
        public override bool ValidateUser(string username, string password)
        {
            try
            {
                var ldap = new LdapServer("<mydomain>:<myport>", "<your ldap search string here>", "uid={0}");
                var user = ldap.Authenticate(username, password);
                
                if(user == null)
                {
                    return false;
                }
            }
            catch (Exception ex)
            {
                LogHelper.Error<MyUserMembershipProvider>(ex.Message, ex);
                return false;
            }

            return true;
        }
    }
}
```

Now create one for members, there is only subtle differences here.  Mostly just the class you are extending:

```c#
using System;
using Umbraco.Core.Logging;
using Umbraco.Web.Security.Providers;
using MyNamespace.Ldap;

namespace MyNamespace
{
    public class MyMemberMembershipProvider : MembersMembershipProvider
    {
        public override bool ValidateUser(string username, string password)
        {
            try
            {
                var ldap = new LdapServer("<mydomain>:<myport>", "<my ldap search string>, "uid={0}");
                var user = ldap.Authenticate(username, password);

                if (user == null)
                {
                    return false;
                }
            }
            catch (Exception ex)
            {
                LogHelper.Error<MyUserMembershipProvider>(ex.Message, ex);
                return false;
            }

            return true;
        }
    }
}
```

Finally we just need to instruct Umbraco to use our providers instead of the built-in ones.  Modify the `Web.Config` and update the provider names:

```
<add name="UmbracoMembershipProvider" type="MyNamespace.MyMemberMembershipProvider, MyAssemblyName" 
minRequiredNonalphanumericCharacters="0" minRequiredPasswordLength="4" useLegacyEncoding="true" enablePasswordRetrieval="false" 
enablePasswordReset="false" requiresQuestionAndAnswer="false" defaultMemberTypeAlias="Member" passwordFormat="Hashed" />

<add name="UsersMembershipProvider" type="MyNamespace.MyUserMembershipProvider, MyAssemblyName" 
minRequiredNonalphanumericCharacters="0" minRequiredPasswordLength="4" useLegacyEncoding="true" enablePasswordRetrieval="false" 
enablePasswordReset="false" requiresQuestionAndAnswer="false" passwordFormat="Hashed" />
```

It's probably best to just comment out the default providers.  Pay close attention to all of the naming.

##Recap
So this setup will allow Umbraco to handle everything except user/password validation.  Membership types and groups all work inside Umbraco as normal.  This is also friendly with the `Public Access` feature of Umbraco.  This setup does not import users from the third-party LDAP.  You will still need to add users (with the same username as LDAP) for them to be able to login.

>You may need to adjust classes based on your LDAP setup as this is a general guide. 

##Use SSL or Risk Security Threats
You should be using SSL for ALL communications handling passwords (backoffice and members).  Since this setup handles passwords being transmitted over the interwebs, mishandling the passwords here could create security issues elsewhere in your system.

You should be using SSL regardless even on a default install.

[<Back 03 - RSS](03 - RSS.md)

[Next> 05 - Image Processor](05 - Image Processor.md)