---
date   2010-10-24 17:00:00
---

Event Sourcing is a different approach to architecting your application.

Instead of persisting the current application state, we store all of the events that led to the that state instead.

When we need to re-create the application state (when the application restarts, for example), we simply replay all of the events.

For example, a typical application might persist a single record like this to a database:

| Article Title | Likes | Social Shares |
|---------|:-------:|:--------:|
| EventSourcing Rocks | 2 | 1 |

Using an Event Sourcing approach, we would instead store something like this:

| Event ID | Event Type | Event Body |
|:--:|------------|------------|
| 1 | Article Created | Title: 'EventSourcing Rocks'|
| 2 | User Liked Article | User: Jane, Article: 'EventSourcing Rocks'|
| 3 | User Shared Article | User: Peter, Article: 'EventSourcing Rocks'|
| 4 | User Liked Article | User: Sam, Article: 'EventSourcing Rocks'|

## Why?

## What is in an event?
The example above is simplified. In reality, there are a few things we want to persists in an event,

## Naming events


------

- What is event sourcing
- Benefits
    - In memory code
    - Easy to replay to debug
- Downsides
    - More storage
    - Longer to start (can use snapshotting)
