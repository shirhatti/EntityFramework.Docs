---
uid: querying/related-data
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Loading Related Data

Entity Framework Core allows you to use the navigation properties in your model to load related entities. There are three common O/RM patterns used to load related data.
   * **Eager loading** means that the related data is loaded from the database as part of the initial query.

   * **Explicit loading** means that the related data is explicitly loaded from the database at a later time.

   * **Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed. Lazy loading is not yet possible with EF Core.

Tip: You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/Querying) on GitHub.

  ## Eager loading

You can use the `Include` method to specify related data to be included in query results. In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blogs = context.Blogs
       .Include(blog => blog.Posts)
       .ToList();

   ````

Tip: Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance. So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.

You can include related data from multiple relationships in a single query.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blogs = context.Blogs
       .Include(blog => blog.Posts)
       .Include(blog => blog.Owner)
       .ToList();

   ````

  ### Including multiple levels

You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method. The following example loads all blogs, their related posts, and the author of each post.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blogs = context.Blogs
       .Include(blog => blog.Posts)
           .ThenInclude(post => post.Author)
       .ToList();

   ````

You can chain multiple calls to `ThenInclude` to continue including further levels of related data.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blogs = context.Blogs
       .Include(blog => blog.Posts)
           .ThenInclude(post => post.Author)
           .ThenInclude(author => author.Photo)
       .ToList();

   ````

You can combine all of this to include related data from multiple levels and multiple roots in the same query.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blogs = context.Blogs
       .Include(blog => blog.Posts)
           .ThenInclude(post => post.Author)
           .ThenInclude(author => author.Photo)
       .Include(blog => blog.Owner)
           .ThenInclude(owner => owner.Photo)
       .ToList();

   ````

  ### Ignored includes

If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.

In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type. In this case, the include operators have no effect.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blogs = context.Blogs
       .Include(blog => blog.Posts)
       .Select(blog => new
       {
           Id = blog.BlogId,
           Url = blog.Url
       })
       .ToList();

   ````

By default, EF Core will log a warning when include operators are ignored. See [Logging](../miscellaneous/logging.md) for more information on viewing logging output. You can change the behavior when an include operator is ignored to either throw or do nothing. This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1, "hl_lines": [5]}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs"} -->

````c#

   protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
   {
       optionsBuilder
           .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
           .ConfigureWarnings(warnings => warnings.Throw(CoreEventId.IncludeIgnoredWarning));
   }

   ````

  ## Explicit loading

Explicit loading does not yet have a first class API in EF Core. You can view the [explicit loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/625) to track this feature.

However, you can use a LINQ query to load the data related to an existing entity instance, by filtering to entities related to the entity in question. Because EF Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance, the loaded data will be populated into the desired navigation property.

In the following example, a query is used to load a blog, and then a later query is used to load the posts related to the blog. The loaded posts will be present in the `Posts` property of the previously loaded blog.

<!-- literal_block {"ids": [], "classes": [], "xml:space": "preserve", "backrefs": [], "linenos": true, "dupnames": [], "language": "c#", "highlight_args": {"linenostart": 1}, "names": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/querying/Querying/Querying/RelatedData/Sample.cs"} -->

````c#

   var blog = context.Blogs
       .Single(b => b.BlogId == 1);

   context.Posts
       .Where(p => p.BlogId == blog.BlogId)
       .Load();

   ````

  ## Lazy loading

Lazy loading is not yet supported by EF Core. You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.
