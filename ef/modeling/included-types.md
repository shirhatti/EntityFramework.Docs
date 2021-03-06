---
uid: modeling/included-types
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Including & Excluding Types

Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.

  ## Conventions

By convention, types that are exposed in `DbSet` properties on your context are included in your model. In addition, types that are mentioned in the `OnModelCreating` method are also included. Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.

For example, in the following code listing all three types are discovered:
   * `Blog` because it is exposed in a `DbSet` property on the context

   * `Post` because it is discovered via the `Blog.Posts` navigation property

   * `AuditEntry` because it is mentioned in `OnModelCreating`

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/Conventions/Samples/IncludedTypes.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [3, 7, 16], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<AuditEntry>();
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }

           public List<Post> Posts { get; set; }
       }

       public class Post
       {
           public int PostId { get; set; }
           public string Title { get; set; }
           public string Content { get; set; }

           public Blog Blog { get; set; }
       }

       public class AuditEntry
       {
           public int AuditEntryId { get; set; }
           public string Username { get; set; }
           public string Action { get; set; }
       }

   ````

  ## Data Annotations

You can use Data Annotations to exclude a type from the model.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/DataAnnotations/Samples/IgnoreType.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [9], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }

           public BlogMetadata Metadata { get; set; }
       }

       [NotMapped]
       public class BlogMetadata
       {
           public DateTime LoadedFromDatabase { get; set; }
       }

   ````

  ## Fluent API

You can use the Fluent API to exclude a type from the model.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/FluentAPI/Samples/IgnoreType.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [7], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Ignore<BlogMetadata>();
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }

           public BlogMetadata Metadata { get; set; }
       }

       public class BlogMetadata
       {
           public DateTime LoadedFromDatabase { get; set; }
       }

   ````
