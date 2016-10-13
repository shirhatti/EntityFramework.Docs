---
uid: modeling/relational/tables
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

Note: The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

  # Table Mapping

Table mapping identifies which table data should be queried from and saved to in the database.

  ## Conventions

By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context. If no `DbSet<TEntity>` is included for the given entity, the class name is used.

  ## Data Annotations

You can use Data Annotations to configure the table that a type maps to.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/DataAnnotations/Samples/Relational/Table.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [1], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       [Table("blogs")]
       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````

You can also specify a schema that the table belongs to.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/DataAnnotations/Samples/Relational/TableAndSchema.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [1], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       [Table("blogs", Schema = "blogging")]
       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````

  ## Fluent API

You can use the Fluent API to configure the table that a type maps to.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/Relational/Table.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [7, 8], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .ToTable("blogs");
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````

You can also specify a schema that the table belongs to.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [2], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

               modelBuilder.Entity<Blog>()
                   .ToTable("blogs", schema: "blogging");

   ````
