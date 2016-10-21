---
uid: modeling/relational/primary-keys
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

Note: The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

  # Primary Keys

A primary key constraint is introduced for the key of each entity type.

  ## Conventions

By convention, the primary key in the database will be named `PK_<type name>`.

  ## Data Annotations

No relational database specific aspects of a primary key can be configured using Data Annotations.

  ## Fluent API

You can use the Fluent API to configure the name of the primary key constraint in the database.

<!-- literal_block {"language": "csharp", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [9], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .HasKey(b => b.BlogId)
                   .HasName("PrimaryKey_BlogId");
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````
