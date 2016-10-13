---
uid: modeling/relational/unique-constraints
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

Note: The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

  # Alternate Keys (Unique Constraints)

A unique constraint is introduced for each alternate key in the model.

  ## Conventions

By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`. For composite alternate keys `<property name>` becomes an underscore separated list of property names.

  ## Data Annotations

Unique constraints can not be configured using Data Annotations.

  ## Fluent API

You can use the Fluent API to configure the index and constraint name for an alternate key.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [9], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

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
