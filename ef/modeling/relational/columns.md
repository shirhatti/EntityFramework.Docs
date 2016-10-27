---
uid: modeling/relational/columns
---
# Column Mapping

> [!WARNING]
> This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

> [!NOTE]
> The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

Column mapping identifies which column data should be queried from and saved to in the database.

## Conventions

By convention, each property will be setup to map to a column with the same name as the property.

## Data Annotations

You can use Data Annotations to configure the column to which a property is mapped.

<!-- [!code-csharp[Main](samples/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
````csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
````

## Fluent API

You can use the Fluent API to configure the column to which a property is mapped.

<!-- [!code-csharp[Main](samples/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
````csharp
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
