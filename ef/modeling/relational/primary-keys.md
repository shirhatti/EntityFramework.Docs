---
uid: modeling/relational/primary-keys
---
# Primary Keys

> [!WARNING]
> This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

> [!NOTE]
> The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

A primary key constraint is introduced for the key of each entity type.

## Conventions

By convention, the primary key in the database will be named `PK_<type name>`.

## Data Annotations

No relational database specific aspects of a primary key can be configured using Data Annotations.

## Fluent API

You can use the Fluent API to configure the name of the primary key constraint in the database.

<!-- [!code-csharp[Main](samples/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
````csharp
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
