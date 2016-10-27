---
uid: modeling/relational/unique-constraints
---
# Alternate Keys (Unique Constraints)

> [!WARNING]
> This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

> [!NOTE]
> The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

A unique constraint is introduced for each alternate key in the model.

## Conventions

By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`. For composite alternate keys `<property name>` becomes an underscore separated list of property names.

## Data Annotations

Unique constraints can not be configured using Data Annotations.

## Fluent API

You can use the Fluent API to configure the index and constraint name for an alternate key.

<!-- [!code-csharp[Main](samples/relational/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?highlight=9)] -->
````csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate)
            .HasName("AlternateKey_LicensePlate");
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
````
