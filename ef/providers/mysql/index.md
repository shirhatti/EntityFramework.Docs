---
uid: providers/mysql/index
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # MySQL (Official)

This database provider allows Entity Framework Core to be used with MySQL. The provider is maintained as part of the [MySQL project](http://dev.mysql.com).

Caution: This provider is pre-release.

Note: This provider is not maintained as part of the Entity Framework Core project. When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.

  ## Install

Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

<!-- literal_block {"ids": [], "xml:space": "preserve", "classes": [], "dupnames": [], "linenos": false, "backrefs": [], "highlight_args": {}, "names": [], "language": "text"} -->

````text

   PM>  Install-Package MySql.Data.EntityFrameworkCore -Pre
   ````

  ## Get Started

See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

  ## Supported Database Engines

   * MySQL

  ## Supported Platforms

   * Full .NET (4.5.1 onwards)

   * .NET Core