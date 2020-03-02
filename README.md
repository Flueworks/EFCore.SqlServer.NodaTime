# EFCore.NodaTime

Adds native support to EntityFrameworkCore for the [NodaTime](https://nodatime.org/) types.

The following types are supported:
* Instant
* OffsetDateTime
* LocalDateTime
* LocalDate
* LocalTime
* Duration

## Usage
To use, simply install the NuGet package:
```shell
Install-Package SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime
```

Then call the `UseNodaTime()` method as part of your SqlServer configuration during the `UseSqlServer` method call:
```csharp
Using Microsoft.EntityFrameworkCore.SqlServer.NodaTime.Extensions;

options.UseSqlServer("your DB Connection",
                    x => x.UseNodaTime());
```
## DATEADD Support
Support for the SQL `DATEADD` function is added via extension methods for the following types:
* Instant

### Supported Methods
* AddYears
* AddMonths
* AddDays
* AddHours
* AddMinutes
* AddSeconds
* AddMilliseconds

```csharp
Using Microsoft.EntityFrameworkCore.SqlServer.NodaTime.Extensions;

// AddYears
await this.Db.RaceResult
    .Where(r => r.StartTime.AddYears(1) >= Instant.FromUtc(2019, 7, 1, 1, 0))
    .ToListAsync();
// Translates to: 
// SELECT [r].[Id], [r].[EndTime], [r].[StartTime], [r].[StartTimeOffset] 
// FROM [RaceResult] AS [r] WHERE DATEADD(year, CAST(1 AS int), [r].[StartTime]) >= '2019-07-01T01:00:00.0000000Z'
```

## DATEPART Support
Support for the SQL `DATEPART` function is added via extension methods for the following types:
* Instant

### Supported Methods
* Year
* Quarter
* Month
* DayOfYear
* Day
* Week
* WeekDay
* Hour
* Minute
* Second
* Millisecond
* Microsecond
* Nanosecond
* TzOffset
* IsoWeek

```csharp
Using Microsoft.EntityFrameworkCore.SqlServer.NodaTime.Extensions;

// Compare the 'Year' DatePart
await this.Db.RaceResult
    .Where(r => r.StartTime.Year() == 2019)
    .ToListAsync();

// Translates to: 
// SELECT [r].[Id], [r].[EndTime], [r].[StartTime], [r].[StartTimeOffset] 
// FROM [RaceResult] AS [r] WHERE DATEPART(year, [r].[StartTime]) = 2019
```