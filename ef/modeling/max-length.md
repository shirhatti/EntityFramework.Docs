---
uid: modeling/max-length
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Maximum Length

Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property. Maximum length only applies to array data types, such as `string` and `byte[]`.

Note: Entity Framework does not do any validation of maximum length before passing data to the provider. It is up to the provider or data store to validate if appropriate. For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.

  ## Conventions

By convention, it is left up to the database provider to choose an appropriate data type for properties. For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data. For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).

  ## Data Annotations

You can use the Data Annotations to configure a maximum length for a property. In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/DataAnnotations/Samples/MaxLength.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [4], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       public class Blog
       {
           public int BlogId { get; set; }
           [MaxLength(500)]
           public string Url { get; set; }
       }

   ````

  ## Fluent API

You can use the Fluent API to configure a maximum length for a property. In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/FluentAPI/Samples/MaxLength.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [7, 8, 9], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .Property(b => b.Url)
                   .HasMaxLength(500);
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````
