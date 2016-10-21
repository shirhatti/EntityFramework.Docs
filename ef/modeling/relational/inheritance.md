---
uid: modeling/relational/inheritance
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

Note: The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

  # Inheritance (Relational Database)

Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.

  ## Conventions

By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern. TPH uses a single table to store the data for all types in the hierarchy. A discriminator column is used to identify which type each row represents.

EF will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).

Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern. The *Discriminator* column identifies which type of *Blog* is stored in each row.

<!-- literal_block {"language": "csharp", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }
           public DbSet<RssBlog> RssBlogs { get; set; }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

       public class RssBlog : Blog
       {
           public string RssUrl { get; set; }
       }

   ````

![image](relational/_static/inheritance-tph-data.png)

  ## Data Annotations

You cannot use Data Annotations to configure inheritance.

  ## Fluent API

You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.

<!-- literal_block {"language": "csharp", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [7, 8, 9, 10], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .HasDiscriminator<string>("blog_type")
                   .HasValue<Blog>("blog_base")
                   .HasValue<RssBlog>("blog_rss");
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

       public class RssBlog : Blog
       {
           public string RssUrl { get; set; }
       }

   ````
