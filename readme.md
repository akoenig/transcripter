# transcripter

A "change data capture" library for MySQL / MariaDB

**Important:** Not ready yet – work in progress

## Motivation

[Change data capture (CDC)](https://en.wikipedia.org/wiki/Change_data_capture) is a nice mechanism for transferring data from one source to another. Imagine a scenario in which you have a service that writes data into a MySQL / MariaDB based database. What if you want to use that data in a different service as well, but in a slightly different format? This is where CDC comes to play. It helps to observe the mutations that happens on the specific records within the database and generates events out of it. An example:

Imagine you have the following user in the `users` entity:

```sh
---------------------
| ID     | Name     |
---------------------
| 1      | André    |
---------------------
```

Updating that user via `UPDATE users SET name = "André König" WHERE ID = 1;` would lead to the following change event when using `transcripter`:

```js
const event = {
    id: 1567,
    schema: "myService"
    domain: "users",
    type: "update",
    payload: {
        before: {
            id: 1,
            name: "André"
        },
        after: {
            id: 1,
            name: "André König"
        }
    }
};
```

This event could be taken and pushed to, let's say, a message broker which puts it onto a queue. From there, a different service could consume the event and generate a completely different data structure out of it. This is especially handy when you want to enforce [CQRS](https://martinfowler.com/bliki/CQRS.html) in order to populate a highly optimized read model.

## Goals

`transcripter` is a module that helps you to observe a database and consume the change events in order to transfer it to different destinations. The main goals are:

* easy to use API surface
* resilient – when you start to observe after a certain time period, it will start where it left off
* handles backpressure
* output sink agnostic – you can decide where to send the change events

## Usage

TBD

## License

MIT © [André König](https://andrekoenig.de)
