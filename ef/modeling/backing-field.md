---
uid: modeling/backing-field
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Backing Fields

When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter). This is useful when there is no property setter, or the setter contains logic that should not be executed when setting initial property values for existing entities being loaded from the database.

Caution: The `ChangeTracker` has not yet been enabled to use backing fields when it needs to set the value of a property. This is only an issue for foreign key properties and generated properties - as the change tracker needs to propagate values into these properties. For these properties, a property setter must still be exposed.[Issue #4461](https://github.com/aspnet/EntityFramework/issues/4461) is tracking enabling the `ChangeTracker` to write to backing fields for properties with no setter.

  ## Conventions

By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order):
   * <propertyName> differing only by case

   * _<propertyName>

   * m_<propertyName>

<!-- literal_block"ids  "classes  "xml:space": "preserve", "backrefs  "linenos": true, "dupnames  : "csharp", highlight_args"linenostart": 1, "h1_lines":3, 7, 8, 9, 10, 11 "names  "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/Conventions/Samples/BackingField.cs" -->

````c#

       public class Blog
       {
           private string _url;

           public int BlogId { get; set; }

           public string Url
           {
               get { return _url; }
               set { _url = value; }
           }
       }

   ````

  ## Data Annotations

Backing fields cannot be configured with data annotations.

  ## Fluent API

There is no top level API for configuring backing fields, but you can use the Fluent API to set annotations that are used to store backing field information.

<!-- literal_block"ids  "classes  "xml:space": "preserve", "backrefs  "linenos": true, "dupnames  : "csharp", highlight_args"linenostart": 1, "h1_lines":7, 8, 9, 15, 18 "names  "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/FluentAPI/Samples/BackingField.cs" -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .Property(b => b.Url)
                   .HasAnnotation("BackingField", "_blogUrl");
           }
       }

       public class Blog
       {
           private string _blogUrl;

           public int BlogId { get; set; }
           public string Url => _blogUrl;
       }

   ````
