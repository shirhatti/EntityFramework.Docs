---
uid: querying/tracking
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Tracking vs. No-Tracking

Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker. If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`. Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.

Tip: You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/Querying) on GitHub.

  ## Tracking queries

By default, queries that return entity types are tracking. This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.

In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Tracking/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
       blog.Rating = 5;
       context.SaveChanges();
   }

   ````

  ## No-tracking queries

No tracking queries are useful when the results are used in a read-only scenario. They are quicker to execute because there is no need to setup change tracking information.

You can swap an individual query to be no-tracking:

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1, "hl_lines": [4]}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Tracking/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blogs = context.Blogs
           .AsNoTracking()
           .ToList();
   }

   ````

You can also change the default tracking behavior at the context instance level:

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1, "hl_lines": [3]}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Tracking/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

       var blogs = context.Blogs.ToList();
   }

   ````

  ## Tracking and projections

Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default. In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1, "hl_lines": [7]}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Tracking/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blog = context.Blogs
           .Select(b =>
               new
               {
                   Blog = b,
                   Posts = b.Posts.Count()
               });
   }

   ````

If the result set does not contain any entity types, then no tracking is performed. In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/Tracking/Sample.cs"} -->

````c#

   using (var context = new BloggingContext())
   {
       var blog = context.Blogs
           .Select(b =>
               new
               {
                   Id = b.BlogId,
                   Url = b.Url
               });
   }

   ````
