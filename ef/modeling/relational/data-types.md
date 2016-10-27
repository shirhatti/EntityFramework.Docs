---
uid: modeling/relational/data-types
---
# Data Types

> [!WARNING]
> This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

> [!NOTE]
> The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

Data type refers to the database specific type of the column to which a property is mapped.

## Conventions

By convention, the database provider selects a data type based on the CLR type of the property. It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.

For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).

## Data Annotations

You can use Data Annotations to specify an exact data type for the column.

<!-- [!code-csharp[Main](samples/relational/Modeling/DataAnnotations/Samples/Relational/DataType.cs?highlight=4)] -->
````csharp
public class Blog
{
    public int BlogId { get; set; }
    [Column(TypeName = "varchar(200)")]
    public string Url { get; set; }
}
````

## Fluent API

You can use the Fluent API to specify an exact data type for the column.

<!-- [!code-csharp[Main](samples/relational/Modeling/FluentAPI/Samples/Relational/DataType.cs?highlight=7,8,9)] -->
````csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasColumnType("varchar(200)");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
````

If you are targeting more than one relational provider with the same model then you probably want to specify a data type for each provider rather than a global one to be used for all relational providers.

<!-- [!code-csharp[Main](samples/relational/Modeling/FluentAPI/Samples/Relational/DataTypeForProvider.cs?highlight=3)] -->
````csharp
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .ForSqlServerHasColumnType("varchar(200)");
````
