---
uid: modeling/relational/columns
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

Note: The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

  # Column Mapping

Column mapping identifies which column data should be queried from and saved to in the database.

  ## Conventions

By convention, each property will be setup to map to a column with the same name as the property.

  ## Data Annotations

You can use Data Annotations to configure the column to which a property is mapped.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [3], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       public class Blog
       {
           [Column("blog_id")]
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````

  ## Fluent API

You can use the Fluent API to configure the column to which a property is mapped.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/Relational/Column.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [7, 8, 9], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .Property(b => b.BlogId)
                   .HasColumnName("blog_id");
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````
