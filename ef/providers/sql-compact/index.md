---
uid: providers/sql-compact/index
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Microsoft SQL Server Compact Edition

This database provider allows Entity Framework Core to be used with SQL Server Compact Edition. The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

Note: This provider is not maintained as part of the Entity Framework Core project. When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.

  ## Install

To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

<!-- literal_block {"ids": [], "xml:space": "preserve", "classes": [], "dupnames": [], "linenos": false, "backrefs": [], "highlight_args": {}, "names": [], "language": "text"} -->

````text

   PM>  Install-Package EntityFrameworkCore.SqlServerCompact40
   ````

To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

<!-- literal_block {"ids": [], "xml:space": "preserve", "classes": [], "dupnames": [], "linenos": false, "backrefs": [], "highlight_args": {}, "names": [], "language": "text"} -->

````text

   PM>  Install-Package EntityFrameworkCore.SqlServerCompact35
   ````

  ## Get Started

See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

  ## Supported Database Engines

   * SQL Server Compact Edition 3.5

   * SQL Server Compact Edition 4.0

  ## Supported Platforms

   * Full .NET (4.5.1 onwards)
