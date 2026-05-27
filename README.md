# Sales.Calendar
Sales.Calendar is the calendar domain module for the Sales backend. It provides business logic and services for managing calendar events, availability windows, recurrence rules, and conflict detection used by the wider Sales platform. The project focuses on domain correctness and integrations (repositories, notifications) rather than UI concerns.

Approximate size

- Lines of code: ≈ 600 LOC (models, services, repositories, controllers, and helpers). This is an approximation and may vary as the project evolves.

Key features

- CRUD operations for calendar events and availability windows
- Support for recurring events (basic recurrence rules)
- Time zone aware event storage and retrieval
- Conflict detection and availability checks
- Reminder/notification hooks (pluggable interfaces to notification service)
- Import/export helpers (iCalendar interoperability points can be added)
- Repository abstraction for persistence so storage can be swapped (EF Core, Dapper, external API)

Services and components

- CalendarService: core business logic for event creation, update, delete, and queries
- ICalendarRepository: persistence abstraction used by services
- Models: Event, Recurrence, Attendee, AvailabilityWindow, Reminder
- DTOs: lightweight shapes for API and integration boundaries
- Time zone helpers and normalization utilities

Configuration

Typical configuration values (appsettings.json):

- Calendar:DefaultTimezone - default timezone identifier (e.g., "UTC")
- Calendar:ReminderWorkerInterval - interval for background reminder checks
- Calendar:Repository:Type - "InMemory" | "EfCore" | "Custom"
- Connection strings or provider-specific settings when using a DB-backed repository

Running and building

Requirements: .NET 11 SDK

Build:
- dotnet build Sales.Calendar

Run tests (if present in sibling test project):
- dotnet test <test-project>

Usage notes

- Services are registered via dependency injection. Repositories and notification adapters should be registered in the host application's composition root.
- The CalendarService is synchronous/async depending on the repository contract; prefer the async contract for I/O-bound implementations.
- Keep time-zone normalization at the service boundary: accept local inputs, normalize for storage, and convert for presentation.

Extensibility

- Add exporters/importers for iCalendar (.ics) to enable cross-system interoperability
- Implement advanced recurrence rules (RRULE) if required by business needs
- Plug in a scheduling or background worker for reminders and notifications

Contributing

- Follow repository coding conventions and add unit tests for new behaviors.

License

- Refer to the repository license at the project root.
