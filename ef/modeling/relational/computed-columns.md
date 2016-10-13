---
uid: modeling/relational/computed-columns
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

Note: The configuration in this section is applicable to relational databases in general. The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).

  # Computed Columns

A computed column is a column whose value is calculated in the database. A computed column can use other columns in the table to calculate its value.

  ## Conventions

By convention, computed columns are not created in the model.

  ## Data Annotations

Computed columns can not be configured with Data Annotations.

  ## Fluent API

You can use the Fluent API to specify that a property should map to a computed column.

<!-- literal_block {"language": "c#", "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs", "xml:space": "preserve", "classes": [], "backrefs": [], "names": [], "dupnames": [], "highlight_args": {"hl_lines": [9], "linenostart": 1}, "ids": [], "linenos": true} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Person> People { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Person>()
                   .Property(p => p.DisplayName)
                   .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
           }
       }

       public class Person
       {
           public int PersonId { get; set; }
           public string FirstName { get; set; }
           public string LastName { get; set; }
           public string DisplayName { get; set; }
       }

   ````
