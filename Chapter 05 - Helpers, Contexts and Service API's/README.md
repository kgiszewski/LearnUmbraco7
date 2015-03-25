#Overview#

Umbraco comes full of helpers, contexts and services.  Before you decide to roll your own implementation of CRUD operations, you should be familiar with some of the basic things Umbraco provides through it's public classes and methods.

For instance, you may be tempted to install and use Entity Framework to handle custom table operations.  You should know that Umbraco uses a micro-ORM named PetaPoco tohandle some basic database operations. 

>For the record you can still use Entity Framework in parallel with PetaPoco.