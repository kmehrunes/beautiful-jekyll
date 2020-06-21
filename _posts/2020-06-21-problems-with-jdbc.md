---
layout: post
title: 'The issues with with JDBC'
tags: [programming, java]
---

Java Database Connectivity, commonly referred to by its acronym JDBC, is probably
my least favorite part of the whole Java ecosystem. I also have issues with JPA
and ORM in general but we can leave that rant for another time.

This article assumes that you know what JDBC is so there will be no introduction.
To summarize the whole article, I hate JDBC. You can leave now if you don't want
to get into the technical details.

Before we start, here are two quotes on JDBC from two of my friends:
> "JDBC feels like the engineering team was too busy doing other things and let
the marketing team develop it." Tao, 2020

> "JDBC was probably designed by the same people who worked on the calendar API
in Java" Rafael, 2020

So I'm not entirely alone in this. That was reassuring.

## Why does it even exist?
JDBC is part of Java, but seriously why? Why should Java come with support for
something external out of the box? If you think this is a ridiculous question
replace SQL with Mongo for example. Should Java ship with support for Mongo? Nope.
But SQL is standardized while each NoSQL database is rolling its own
specifications. Still, the question stands: *why should Java come with its own
support for SQL DBs?*. They should have left the creators of each DB to write
their own drivers, they already do that.

I would argue that including it does more harm than good as it forces databases to
implement their drivers according to JDBC instead of each database driver having
the freedom to design its own interface. So what? They can still do that, right?
Technically yes but your users will prefer easier compatibility and familiarity and
since they have been using JDBC they'll prefer something which is JDBC-compliant.

## Template Statements
My biggest problem with JDBC is its template statements. Which were probably
introduced as a troll but somehow made it into the final release. Let's say that
you want to select a record based on its ID. You'd think that JDBC supports named
variables so that you can have your statement looking like this

```sql
SELECT * FROM some_table WHERE id = ${id}
```

But you will be out of luck because your statement has to look like this

```sql
SELECT * FROM some_table WHERE id = ?
```

And then you use a setter method to set the value. Not too bad. What if you have
multiple variables? A statement like this for example

```sql
INSERT INTO some_table VALUES(?, ?, ?)
```

How are you going to set the values? Well by index obviously. Yes, you need to know
the order in which your placeholders are placed in the statement. So now you need
to do

```java
statement.setObject(0, id);
statement.setObject(1, name); … etc
```

Not so fast. Indices in JDBC start from 1. Yes, everything in Java starts from 0
but JDBC starts from 1. What a mess!

## No Async for You
We'll keep this one short and concise cause it's all in the title. You can be
sneaky and use a `CompletableFuture` but the call inside the future will block
whatever thread it's running on. Thankfully we have [Vertx now](https://vertx.io/blog/using-the-asynchronous-sql-client/) (*disclaimer: I didn't try it*)

## Conclusion
JDBC was a joke which was taken too far and now we all have to deal with it
