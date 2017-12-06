---
title: La gestione della concorrenza - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a>Gestione della concorrenza

Se una proprietà è configurata come un token di concorrenza EF verificherà che nessun altro utente ha modificato il valore nel database durante il salvataggio delle modifiche al record specifico.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) su GitHub.

## <a name="how-concurrency-handling-works-in-ef-core"></a>Il funzionamento di gestione delle concorrenze in EF Core

Per una descrizione dettagliata del funzionamento di gestione delle concorrenze in Entity Framework Core, vedere [i token di concorrenza](../modeling/concurrency.md).

## <a name="resolving-concurrency-conflicts"></a>Risoluzione dei conflitti di concorrenza

Risoluzione di un conflitto di concorrenza prevede l'utilizzo di un algoritmo di unire le modifiche in sospeso da parte dell'utente corrente con le modifiche apportate nel database. L'esatto approccio variano in base all'applicazione, ma un approccio comune consiste nel visualizzare i valori per l'utente e farli decidere i valori corretti da archiviare nel database.

**Sono disponibili tre set di valori disponibili per risolvere un conflitto di concorrenza.**

* **I valori correnti** sono i valori che l'applicazione ha tentato di scrivere nel database.

* **I valori originali** sono i valori che sono stati originariamente recuperati dal database, prima di apportare eventuali modifiche.

* **I valori del database** sono i valori attualmente archiviati nel database.

Per gestire un conflitto di concorrenza, è possibile intercettare un `DbUpdateConcurrencyException` durante `SaveChanges()`, utilizzare `DbUpdateConcurrencyException.Entries` per preparare un nuovo set di modifiche per le entità interessate, quindi ripetere il `SaveChanges()` operazione.

Nell'esempio seguente, `Person.FirstName` e `Person.LastName` siano impostati come token di concorrenza. È presente un `// TODO:` commento nella posizione in cui è necessario includere la logica specifica dell'applicazione per scegliere il valore da salvare nel database.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
