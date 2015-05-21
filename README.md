#Learn Umbraco 7#

![7377960988_7c087be22e_o.jpg](assets/7377960988_7c087be22e_o.jpg)
>Photo by: Doug Robar

**Work in Progress  - Sections updated regularly.**

Learn Umbraco 7 is a crowd authored book with the purpose of onboarding new Umbraco developers for free.  This book isn't intended to be a resource guide, please refer to the official Umbraco documentation for that.  This book is a narrative of different topics in the Umbraco 7 realm.

This book is downloadable despite not being complete.  This is being done to encourage more authors to fill in the blanks.  Please consider writing a section :)

You can view this book right inside the Umbraco 7 backoffice using [Umbraco Bookshelf](https://our.umbraco.org/projects/backoffice-extensions/bookshelf).  You can also install Bookshelf with [NuGet](https://www.nuget.org/packages/UmbracoBookshelf/).

##Target Audience##
This book is targeted for readers who:

* Have developed web applications in other languages
* Have little or no experience with Umbraco
* Have some experience with .NET
* Have some experience with the client languages Javascript, HTML and CSS

This book is not intended for readers who:

* Have no web developing experience
* Are looking for an editor (user) perspective

##Targeted Skills and Technology##
* Installation of developer tools: Visual Studio Web Express
* Installation of Umbraco via NuGet
* Installation and development of plugins
* Common Umbraco Patterns
* Common Umbraco Anti-patterns
* Client languages and frameworks: HTML, Javascript, CSS, AngularJs, Underscore.js
* Server languages and frameworks: C#, ASP.NET, MVC
* Data: SQL CE, MSSQL Express and SSMS
* Hosting: Internet Information Services (IIS), Win Server

##License##
Content in this repository is freely available to read and use for non-commercial uses. It may not be reproduced or used for commercial use without consent. All rights are reserved and copyrighted by the contributors.  All images and files are copyrighted by their respective owners.  All logos are trademarked by their respective owners.  Please seek permission to use before using any materials for any other purposes.

##Contribute##
If you wish to contribute to the book, you may do so by submitting a pull request to this repository. You can send us a simple spelling correction, a section or even an entire chapter. By contributing you agree in full to the Contributor Agreement described below.

##Contributor Agreement##
The purpose of this book is to provide free information to those who want to know. By submitting any content, you affirm and agree the following:

* The contribution is completely original and not taken from any source from which you do not possess the copyright 
* You agree to allow your contribution to be used for any purpose by the repository owners in a non-commercial way

##Style Guide##

####Structure####
The structure of this books is the following:

* `Chapter` - Chapters are folders named as `{Chapter Number} - {Chapter Name}` 
    * `Section` - A section is a markdown file named as `{Section Number} - {Section Name}.md`
    * `Assets` - A folder for media (to hold images/pdfs for this chapter) named as `assets`
    * `Readme.md` - All folders are required to have one and this serves as an overview page

####Do Use Markdown####
Everything should be written in markdown and not HTML.  If you are not familiar with markdown, please use these references:

* https://help.github.com/articles/markdown-basics/
* https://help.github.com/articles/github-flavored-markdown/

####Do Use Root Relative, Current Relative or External Links####
Use links that are rooted from the top level like so `[click me](/LearnUmbraco7/01 - Chapter 0/readme.md)`.

####Do Not Use Relative Paths with Double Dots####
These type of links `[click me](../01 - Chapter 0/readme.md)` are useful normally, but create a security issue when downloaded into things like [Umbraco Bookshelf](https://github.com/kgiszewski/UmbracoBookshelf).
>Side note, Umbraco Bookshelf ignores these types of links.

####Do Split Things Up####
Try to split chapters into logical sections.

####Do Use the Assets Folder####
Each chapter has an `assets` folder where your pdf and image files should go.

When linking assets, use relative paths like `![my image](assets/myimage.png)`.

####Do Use Blockquotes for Callouts####
Need to call something out?  Use a blockquote.

####Do Use Code Literals and Namespaces####
Use code blocks when showing code.  Always include namespaces for C#.

##So what should you contribute?##
Each chapter and section will has TODO lists associated with them as well as general outlines of future content. Feel free to tackle any of those or submit other TODOs.  Here are some other examples of what you can contribute:

* Sample Code
* Diagrams
* Photos from the Umbraco World
* Editing by reading, correcting and clarifying; if it's wrong say so :)

Each person that has been assimilated into Umbraco has a unique opportunity to contribute in some way.

##A Note to Grammar Nazis##
Writing narrative text is not our best skill so if you see an issue with overall organization, grammar, spelling and/or other issues; we expect a pull-request to fix the issues :)
