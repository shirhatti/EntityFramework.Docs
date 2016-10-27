---
uid: modeling/relational/default-schema
---
# Default Schema

> [!WARNING]
> This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

> [!NOTE]
> The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.

## Conventions

By convention, the database provider will choose the most appropriate default schema. For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).

## Data Annotations

You can not set the default schema using Data Annotations.

## Fluent API

You can use the Fluent API to specify a default schema.

<!-- [!code-csharp[Main](samples/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
````csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
````
