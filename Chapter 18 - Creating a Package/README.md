#Creating a Package#

So you want to create a package? Before we get into the gooey bits, let's define a package.  A package in Umbraco is a simple `zip` file that contains files to be placed on a system. It includes one special file called the manifest.

There are two main ways to distribute something that you've made to the world, each with their own advantages:

* Use the built-in packager found in `Developer->Packages`
* Build a NuGet package