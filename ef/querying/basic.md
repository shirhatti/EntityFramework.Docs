---
uid: querying/basic
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Basic Query

Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database. LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.

Tip: You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/Querying) on GitHub.

  ## 101 LINQ samples

This page shows a few examples to achieve common tasks with Entity Framework Core. For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

  ## Loading all data

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Basics/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blogs = context.Blogs.ToList();
   }

   ````

  ## Loading a single entity

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Basics/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blog = context.Blogs
           .Single(b => b.BlogId == 1);
   }

   ````

  ## Filtering

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Basics/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blogs = context.Blogs
           .Where(b => b.Url.Contains("dotnet"))
           .ToList();
   }

   ````
