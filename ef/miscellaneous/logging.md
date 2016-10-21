---
uid: miscellaneous/logging
---
Caution: This documentation is for EF Core. For EF6.x and earlier release see [http://msdn.com/data/ef](http://msdn.com/data/ef).

  # Logging

Tip: You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/Miscellaneous/Logging) on GitHub.

  ## Create a logger

The first step is to create an implementation of `ILoggerProvider` and `ILogger`.
   * `ILoggerProvider` is the component that decides when to create instances of your logger(s). The provider may choose to create different loggers in different situations.

   * `ILogger` is the component that does the actual logging. It will be passed information from the framework when certain events occur.

Here is a simple implementation that logs a human readable representation of every event to a text file and the Console.

<!-- literal_block"ids  "classes  "xml:space": "preserve", "backrefs  "linenos": true, "dupnames  : "csharp",rp", "names  "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/miscellaneous/Miscellaneous/Logging/Logging/MyLoggerProvider.cs" -->

````csharp

   using Microsoft.Extensions.Logging;
   using System;
   using System.IO;

   namespace EFLogging
   {
       public class MyLoggerProvider : ILoggerProvider
       {
           public ILogger CreateLogger(string categoryName)
           {
               return new MyLogger();
           }

           public void Dispose()
           { }

           private class MyLogger : ILogger
           {
               public bool IsEnabled(LogLevel logLevel)
               {
                   return true;
               }

               public void Log<TState>(LogLevel logLevel, EventId eventId, TState state, Exception exception, Func<TState, Exception, string> formatter)
               {
                   File.AppendAllText(@"C:\temp\log.txt", formatter(state, exception));
                   Console.WriteLine(formatter(state, exception));
               }

               public IDisposable BeginScope<TState>(TState state)
               {
                   return null;
               }
           } 
       }
   }

   ````

Tip:

  The arguments passed to the Log method are:
     * `logLevel` is the level (e.g. Warning, Info, Verbose, etc.) of the event being logged

     * `eventId` is a library/assembly specific id that represents the type of event being logged

     * `state` can be any object that holds state relevant to what is being logged

     * `exception` gives you the exception that occurred if an error is being logged

     * `formatter` uses state and exception to create a human readable string to be logged

  ## Register your logger  ### ASP.NET Core

In an ASP.NET Core application, you register your logger in the Configure method of Startup.cs:

<!-- literal_block"ids  "classes  "xml:space": "preserve", "backrefs  "dupnames  "names": [] -->

````

   public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
   {
       loggerFactory.AddProvider(new MyLoggerProvider());

       ...
   }
   ````

  ### Other applications

In your application startup code, create and instance of you context and register your logger.

Note: You only need to register the logger with a single context instance. Once you have registered it, it will be used for all other instances of the context in the same AppDomain.

<!-- literal_block"ids  "classes  "xml:space": "preserve", "backrefs  "linenos": true, "dupnames  : "csharp",rp", "names  "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/miscellaneous/Miscellaneous/Logging/Logging.ConsoleApp/Program.cs" -->

````csharp

               using (var db = new BloggingContext())
               {
                   var serviceProvider = db.GetInfrastructure<IServiceProvider>();
                   var loggerFactory = serviceProvider.GetService<ILoggerFactory>();
                   loggerFactory.AddProvider(new MyLoggerProvider());
               }

   ````

  ## Filtering what is logged

The easiest way to filter what is logged, is to adjust your logger provider to only return your logger for certain categories of events. For EF, the category passed to your logger provider will be the type name of the component that is logging the event.

For example, here is a logger provider that returns the logger only for events related to executing SQL against a relational database. For all other categories of events, a null logger (which does nothing) is returned.

<!-- literal_block"ids  "classes  "xml:space": "preserve", "backrefs  "linenos": true, "dupnames  : "csharp",rp", highlight_args"linenostart": 1, "h1_lines":9, 10, 11, 12, 13, 17, 18, 19, 20, 21, 22 "names  "source": "/Users/shirhatti/src/EntityFramework.Docs/docs/miscellaneous/Miscellaneous/Logging/Logging/MyFilteredLoggerProvider.cs" -->

````csharp

   using Microsoft.Extensions.Logging;
   using System;
   using System.Linq;

   namespace EFLogging
   {
       public class MyFilteredLoggerProvider : ILoggerProvider
       {
           private static string[] _categories =
           {
               typeof(Microsoft.EntityFrameworkCore.Storage.Internal.RelationalCommandBuilderFactory).FullName,
               typeof(Microsoft.EntityFrameworkCore.Storage.Internal.SqlServerConnection).FullName
           };

           public ILogger CreateLogger(string categoryName)
           {
               if( _categories.Contains(categoryName))
               {
                   return new MyLogger();
               }

               return new NullLogger();
           }

           public void Dispose()
           { }

           private class MyLogger : ILogger
           {
               public bool IsEnabled(LogLevel logLevel)
               {
                   return true;
               }

               public void Log<TState>(LogLevel logLevel, EventId eventId, TState state, Exception exception, Func<TState, Exception, string> formatter)
               {
                   Console.WriteLine(formatter(state, exception));
               }

               public IDisposable BeginScope<TState>(TState state)
               {
                   return null;
               }
           }

           private class NullLogger : ILogger
           {
               public bool IsEnabled(LogLevel logLevel)
               {
                   return false;
               }

               public void Log<TState>(LogLevel logLevel, EventId eventId, TState state, Exception exception, Func<TState, Exception, string> formatter)
               { }

               public IDisposable BeginScope<TState>(TState state)
               {
                   return null;
               }
           }
       }
   }

   ````
