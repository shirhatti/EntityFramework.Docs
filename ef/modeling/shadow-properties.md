---
uid: modeling/shadow-properties
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Shadow Properties

Shadow properties are properties that do not exist in your entity class. The value and state of these properties is maintained purely in the Change Tracker.

Shadow property values can be obtained and changed through the `ChangeTracker` API.

<!-- literal_block {"ids": [], "xml:space": "preserve", "classes": [], "dupnames": [], "linenos": false, "backrefs": [], "highlight_args": {}, "names": [], "language": "csharp"} -->

````csharp

   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
   ````

Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.

<!-- literal_block {"ids": [], "xml:space": "preserve", "classes": [], "dupnames": [], "linenos": false, "backrefs": [], "highlight_args": {}, "names": [], "language": "csharp"} -->

````csharp

   var blogs = context.Blogs
       .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
   ````

  ## Conventions

By convention, shadow properties are only created when a relationship is discovered but no foreign key property is found in the dependent entity class. In this case, a shadow foreign key property will be introduced. The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming). If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`. If there is no navigation property on the dependent entity, then the principal type name is used in its place.

For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/Conventions/Samples/ShadowForeignKey.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }
           public DbSet<Post> Posts { get; set; }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }

           public List<Post> Posts { get; set; }
       }

       public class Post
       {
           public int PostId { get; set; }
           public string Title { get; set; }
           public string Content { get; set; }

           public Blog Blog { get; set; }
       }

   ````

  ## Data Annotations

Shadow properties can not be created with data annotations.

  ## Fluent API

You can use the Fluent API to configure shadow properties. Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.

If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.

<!-- literal_block {"ids": [], "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/modeling/Modeling/FluentAPI/Samples/ShadowProperty.cs", "classes": [], "dupnames": [], "linenos": true, "backrefs": [], "highlight_args": {"hl_lines": [7, 8], "linenostart": 1}, "language": "c#", "names": [], "xml:space": "preserve"} -->

````c#

       class MyContext : DbContext
       {
           public DbSet<Blog> Blogs { get; set; }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Blog>()
                   .Property<DateTime>("LastUpdated");
           }
       }

       public class Blog
       {
           public int BlogId { get; set; }
           public string Url { get; set; }
       }

   ````