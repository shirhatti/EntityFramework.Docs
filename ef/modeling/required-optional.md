---
uid: modeling/required-optional
---
# Required/optional properties

> [!WARNING]
> This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

A property is considered optional if it is valid for it to contain `null`. If `null` is not a valid value to be assigned to a property then it is considered to be a required property.

## Conventions

By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.). Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).

> [!NOTE]
> A property whose CLR type cannot contain null cannot be configured as optional. The property will always be considered required by Entity Framework.

## Data Annotations

You can use Data Annotations to indicate that a property is required.

<!-- [!code-csharp[Main](samples/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
````csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
````

## Fluent API

You can use the Fluent API to indicate that a property is required.

<!-- [!code-csharp[Main](samples/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
````csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
````
