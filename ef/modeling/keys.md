---
uid: modeling/keys
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Keys (primary)

A key serves as the primary unique identifier for each entity instance. When using a relational database this maps to the concept of a *primary key*. You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).

  ## Conventions

By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/Conventions/Samples/KeyId.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [3], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class Car
       {
           public string Id { get; set; }

           public string Make { get; set; }
           public string Model { get; set; }
       }

   ````

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/Conventions/Samples/KeyTypeNameId.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [3], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class Car
       {
           public string CarId { get; set; }

           public string Make { get; set; }
           public string Model { get; set; }
       }

   ````

  ## Data Annotations

You can use Data Annotations to configure a single property to be the key of an entity.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/DataAnnotations/Samples/KeySingle.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [3, 4], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class Car
       {
           [Key]
           public string LicensePlate { get; set; }

           public string Make { get; set; }
           public string Model { get; set; }
       }

   ````

  ## Fluent API

You can use the Fluent API to configure a single property to be the key of an entity.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/FluentAPI/Samples/KeySingle.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [7, 8], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Car> Cars { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Car>()
                   .HasKey(c => c.LicensePlate);
           }
       }

       class Car
       {
           public string LicensePlate { get; set; }

           public string Make { get; set; }
           public string Model { get; set; }
       }

   ````

You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key). Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/FluentAPI/Samples/KeyComposite.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [7, 8], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Car> Cars { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Car>()
                   .HasKey(c => new { c.State, c.LicensePlate });
           }
       }

       class Car
       {
           public string State { get; set; }
           public string LicensePlate { get; set; }

           public string Make { get; set; }
           public string Model { get; set; }
       }

   ````
